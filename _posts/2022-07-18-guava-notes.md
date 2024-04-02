---
layout: post
title: "Guava Notes"
date: 2022-07-18 09:57:00 +0800
categories: Java
---
## Lists

1. New list
```java
List<String> names = Lists.newArrayList("John", "Adam", "Jane");
```
2. Reverse List

```java
@Test
public void whenReverseList_thenReversed() {
    List<String> names = Lists.newArrayList("John", "Adam", "Jane");

    List<String> reversed = Lists.reverse(names);
    assertThat(reversed, contains("Jane", "Adam", "John"));
}
```

3. Generate Character List from a String

```java
@Test
public void whenCreateCharacterListFromString_thenCreated() {
    List<Character> chars = Lists.charactersOf("John");

    assertEquals(4, chars.size());
    assertThat(chars, contains('J', 'o', 'h', 'n'));
}
```

4. Partition a List

```java
@Test
public void whenPartitionList_thenPartitioned(){
    List<String> names = Lists.newArrayList("John","Jane","Adam","Tom","Viki","Tyler");

    List<List<String>> result = Lists.partition(names, 2);

    assertEquals(3, result.size());
    assertThat(result.get(0), contains("John", "Jane"));
    assertThat(result.get(1), contains("Adam", "Tom"));
    assertThat(result.get(2), contains("Viki", "Tyler"));
}
```

5. Remove Duplicates From List

```java

@Test
public void whenRemoveDuplicatesFromList_thenRemoved() {
    List<Character> chars = Lists.newArrayList('h','e','l','l','o');
    assertEquals(5, chars.size());

    List<Character> result = ImmutableSet.copyOf(chars).asList();
    assertThat(result, contains('h', 'e', 'l', 'o'));
}
```

6. Remove Null Values from List

```java
@Test
public void whenRemoveNullFromList_thenRemoved() {
    List<String> names = Lists.newArrayList("John", null, "Adam", null, "Jane");
    Iterables.removeIf(names, Predicates.isNull());

    assertEquals(3, names.size());
    assertThat(names, contains("John", "Adam", "Jane"));
}
```

7. Convert a List to an ImmutableList

```java
@Test
public void whenCreateImmutableList_thenCreated() {
    List<String> names = Lists.newArrayList("John", "Adam", "Jane");

    names.add("Tom");
    assertEquals(4, names.size());

    ImmutableList<String> immutable = ImmutableList.copyOf(names);
    assertThat(immutable, contains("John", "Adam", "Jane", "Tom"));
}
```