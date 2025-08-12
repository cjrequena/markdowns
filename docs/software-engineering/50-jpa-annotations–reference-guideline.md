---
layout: default
title: jpa-annotations–reference-guideline
parent: software-engineering
nav_order: 50
---

# JPA Annotations Reference Guideline
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Abstract

This reference document provides a comprehensive overview of the relationship annotations in Java Persistence API (JPA), focusing on `@ManyToOne` and including all other core relationship types: `@OneToOne`, `@OneToMany`, and `@ManyToMany`. Each annotation is explained with its purpose, use cases, and complete property specifications. It details the behaviour and configuration of relationship mappings, including fetch strategies (`FetchType`), cascading operations (`CascadeType`), and join management (`@JoinColumn`, `@JoinTable`).

Additionally, the document outlines best practices for performance optimization and entity integrity (e.g., using `LAZY` fetch type, orphan removal, and bidirectional mapping with `mappedBy`). The guide serves both as a practical tutorial and a quick reference for developers designing relational data models using JPA.

---



## 🔗 `@ManyToOne`

A `@ManyToOne` relationship maps a many-to-one association: **many instances of one entity are associated with one instance of another entity**.


```java
@ManyToOne(
    optional = true,
    cascade = {},
    fetch = FetchType.EAGER,
    targetEntity = Class.class
)
```

### Properties

| Property       | Type            | Default          | Description                                                                     |
| -------------- | --------------- | ---------------- | ------------------------------------------------------------------------------- |
| `optional`     | `boolean`       | `true`           | Indicates whether the association is optional (nullable foreign key).           |
| `cascade`      | `CascadeType[]` | `{}`             | Specifies operations that should be cascaded to the associated entity.          |
| `fetch`        | `FetchType`     | `EAGER`          | Defines whether the association should be eagerly or lazily loaded.             |
| `targetEntity` | `Class<?>`      | Default inferred | Used to specify the class of the target entity if not inferred (rarely needed). |


**Example:**

Many `Order` entities relate to one `Customer`.

```
+----------+       FK       +-----------+
|  Order   |--------------->| Customer  |
+----------+                +-----------+
| id       |                | id        |
| customer_id (FK)          | name      |
+----------+                +-----------+
```

```java
@Entity
public class Order {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    private Customer customer;
}
```

```java
@Entity
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
}
```

**How it Works:**

* JPA will automatically add a foreign key column (`customer_id`) in the `Order` table referencing the `Customer`'s primary key.
* You can customize the join column using `@JoinColumn`.

```java
@ManyToOne
@JoinColumn(name = "customer_id", nullable = false)
private Customer customer;
```

---

## 📎 `@OneToMany`

`@OneToMany` One entity has a collection of many instances of another.

```java
@OneToMany(
    mappedBy = "",
    cascade = {},
    fetch = FetchType.LAZY,
    orphanRemoval = false,
    targetEntity = Class.class
)
```

### Properties

| Property        | Type            | Default                     | Description                                                             |
| --------------- | --------------- | --------------------------- | ----------------------------------------------------------------------- |
| `mappedBy`      | `String`        | Required (if bidirectional) | Name of the field in the **owning entity** that maps this relationship. |
| `cascade`       | `CascadeType[]` | `{}`                        | Cascade operations to children (e.g., persist, remove).                 |
| `fetch`         | `FetchType`     | `LAZY`                      | Lazy loading is recommended for collections.                            |
| `orphanRemoval` | `boolean`       | `false`                     | Automatically delete child entities if removed from the collection.     |
| `targetEntity`  | `Class<?>`      | Default inferred            | Explicitly declare target class.                                        |


**Example:**

```
+-----------+                +----------+
| Customer  |<--------------+|  Order   |
+-----------+    1       ∞   +----------+
| id        |                | id       |
| name      |                | customer_id (FK) 
+-----------+                +----------+
```

* `Customer` has many `Orders`.
* `Order.customer_id` is the foreign key.

```java
@Entity
public class Customer {
    @OneToMany(mappedBy = "customer")
    private List<Order> orders;
}
```

Usually, this is the **inverse** of a `@ManyToOne`.

---

## 🔗 `@OneToOne`

```java
@OneToOne(
    mappedBy = "",
    cascade = {},
    fetch = FetchType.EAGER,
    optional = true,
    orphanRemoval = false,
    targetEntity = Class.class
)
```

### Properties

| Property        | Type            | Default                     | Description                                      |
| --------------- | --------------- | --------------------------- | ------------------------------------------------ |
| `mappedBy`      | `String`        | Required (if bidirectional) | Field name on the owning side.                   |
| `cascade`       | `CascadeType[]` | `{}`                        | Cascade operations to the related entity.        |
| `fetch`         | `FetchType`     | `EAGER`                     | Whether to load immediately or on access.        |
| `optional`      | `boolean`       | `true`                      | If false, a non-nullable foreign key is created. |
| `orphanRemoval` | `boolean`       | `false`                     | Deletes the associated entity when dereferenced. |
| `targetEntity`  | `Class<?>`      | Default inferred            | Used when generic types are involved.            |


**Example:**

```
+--------+     1:1      +----------+
|  User  |------------->| Profile  |
+--------+              +----------+
| id     |              | id       |
| profile_id (FK, unique)           |
+--------+              +----------+
```
* One `User` links to one `Profile`.
* Foreign key in `User`, marked `UNIQUE` to enforce 1:1.

> Alternatively, the FK could reside in `Profile`, depending on ownership.

```java
@Entity
public class User {
    @OneToOne
    @JoinColumn(name = "profile_id")
    private Profile profile;
}
```


---

## 🔗 `@ManyToMany`

`@ManyToMany` Each entity in the relationship can relate to many instances of the other.

```java
@ManyToMany(
    mappedBy = "",
    cascade = {},
    fetch = FetchType.LAZY,
    targetEntity = Class.class
)
```

### Properties

| Property       | Type            | Default                     | Description                              |
| -------------- | --------------- | --------------------------- | ---------------------------------------- |
| `mappedBy`     | `String`        | Required (if bidirectional) | Specifies the field on the inverse side. |
| `cascade`      | `CascadeType[]` | `{}`                        | Cascade actions to the related entities. |
| `fetch`        | `FetchType`     | `LAZY`                      | Lazy fetching for collections.           |
| `targetEntity` | `Class<?>`      | Default inferred            | Target class (typically inferred).       |


**Example:**

```
+----------+         +------------------+         +--------+
| Student  |         | student_course   |         | Course |
+----------+         +------------------+         +--------+
| id       |         | student_id (FK)  |<------->| id     |
| name     |<------->| course_id (FK)   |         | title  |
+----------+         +------------------+         +--------+
```

* Join table `student_course` handles many-to-many association.


```java
@Entity
public class Student {
    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private Set<Course> courses;
}
```

---

## 📦 CascadeType Enum

Used in: `cascade` property

```java
public enum CascadeType {
    ALL,
    PERSIST,
    MERGE,
    REMOVE,
    REFRESH,
    DETACH
}
```

| CascadeType | Description                                        |
| ----------- | -------------------------------------------------- |
| `ALL`       | Applies all cascade operations.                    |
| `PERSIST`   | Propagate `EntityManager.persist()` to the target. |
| `MERGE`     | Propagate `EntityManager.merge()`.                 |
| `REMOVE`    | Propagate `EntityManager.remove()`.                |
| `REFRESH`   | Propagate `EntityManager.refresh()`.               |
| `DETACH`    | Propagate `EntityManager.detach()`.                |

---

## 🧵 FetchType Enum

Used in: `fetch` property

```java
public enum FetchType {
    LAZY,
    EAGER
}
```

| FetchType | Description                                |
| --------- | ------------------------------------------ |
| `LAZY`    | Association is fetched only when accessed. |
| `EAGER`   | Association is fetched immediately.        |

> ⚠️ **Best Practice**: Use `LAZY` for collections to avoid performance issues.

---

## 🔗 `@JoinColumn`

Used with `@ManyToOne`, `@OneToOne`

```java
@JoinColumn(
    name = "column_name",
    referencedColumnName = "id",
    unique = false,
    nullable = true,
    insertable = true,
    updatable = true
)
```

| Property               | Type      | Default  | Description                       |
| ---------------------- | --------- | -------- | --------------------------------- |
| `name`                 | `String`  | Inferred | Name of the foreign key column.   |
| `referencedColumnName` | `String`  | `"id"`   | Column in the referenced table.   |
| `unique`               | `boolean` | `false`  | Makes the column unique.          |
| `nullable`             | `boolean` | `true`   | Whether the column allows nulls.  |
| `insertable`           | `boolean` | `true`   | Whether the column is insertable. |
| `updatable`            | `boolean` | `true`   | Whether the column is updatable.  |

---

## 🔗 `@JoinTable`

Used with `@ManyToMany`, and optionally `@OneToMany`

```java
@JoinTable(
    name = "join_table_name",
    joinColumns = @JoinColumn(name = "entity_id"),
    inverseJoinColumns = @JoinColumn(name = "related_entity_id")
)
```

| Property             | Type            | Description                                         |
| -------------------- | --------------- | --------------------------------------------------- |
| `name`               | `String`        | Name of the join table.                             |
| `joinColumns`        | `@JoinColumn[]` | Defines the foreign key to the owning entity.       |
| `inverseJoinColumns` | `@JoinColumn[]` | Defines the foreign key to the inverse side entity. |

---

## 📝 Summary Cheatsheet

| Annotation    | Key Props                                   | Default Fetch | Required if bidirectional |
| ------------- | ------------------------------------------- | ------------- | ------------------------- |
| `@ManyToOne`  | `optional`, `cascade`, `fetch`              | `EAGER`       | No                        |
| `@OneToMany`  | `mappedBy`, `cascade`, `orphanRemoval`      | `LAZY`        | Yes                       |
| `@OneToOne`   | `mappedBy`, `optional`, `cascade`           | `EAGER`       | Yes                       |
| `@ManyToMany` | `mappedBy`, `cascade`                       | `LAZY`        | Yes                       |
| `@JoinColumn` | `name`, `referencedColumnName`, etc.        | N/A           | No                        |
| `@JoinTable`  | `name`, `joinColumns`, `inverseJoinColumns` | N/A           | No                        |

---
