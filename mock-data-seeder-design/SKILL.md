---
name: mock-data-seeder-design
description: Designs and writes comprehensive seeders, factories, and realistic mock data setups for Laravel Eloquent, Flutter mock service layers, or API contracts. Enables rapid testing and prototyping.
---

# Mock Data & Seeder Design

This skill provides patterns and instructions for generating realistic mock data, building reliable database seeders and model factories in Laravel, and structuring mock data layers in Flutter.

## Mocking Principles

1. **Realistic Schemas & Cohesive Domains**:
   - Create mock data that mimics actual business scenarios (e.g., realistic names, emails, dates, amounts, and nested relations) instead of generic strings like `test_1`, `test_2`.
   - Ensure linked tables/entities have corresponding relational records (e.g., if a user has posts, seed posts for that specific user).

2. **Laravel Seeding & Factory Patterns**:
   - Use Laravel Model Factories with the `Faker` library for single models.
   - Implement factory states (e.g., `$user->active()`, `$order->shipped()`) to support diverse testing situations.
   - Use Database Seeders to orchestrate the insertion flow, handling dependencies between seed tables, unique constraint checks, and foreign key orders.
   - Use chunked inserts or raw queries if seeding millions of records for load testing.

3. **Flutter Mock Data Providers**:
   - Build offline-first implementations of repositories.
   - Create local JSON data assets to mock network payloads.
   - Use Dart classes that extend abstract repositories and simulate network delay:
     ```dart
     class MockUserRepository implements UserRepository {
       @override
       Future<User> getUser() async {
         await Future.delayed(const Duration(milliseconds: 500));
         return User(id: '1', name: 'John Doe', email: 'john@example.com');
       }
     }
     ```
