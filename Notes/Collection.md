<!-- TOC -->

- [一、java集合框架概述](#一java集合框架概述)
    - [Set](#set)
    - [List](#list)
    - [Queue](#queue)
    - [Map](#map)
- [二、源码分析](#二源码分析)
    - [HashMap](#hashmap)
        - [1、数据结构](#1数据结构)
    - [HashSet](#hashset)

<!-- /TOC -->
# 一、java集合框架概述 #

*java集合框架支持以下两种类型的容器：*

一种为了存储一个元素集合，简称集合(Collection):

一种是为了存储键/值对，称为图(Map):

![Collection集合框架](https://github.com/Aroue/Lee-notes/blob/master/images/Collection.png)

* Set：用于存储一组不重复的元素
* List：由元素构成的有序的集合
* Queue：用于存储用先进先出方式处理的对象
* map: 一种依照键/值对存储元素的容器

## Set ##
* HashSet: hashSet类是实现了Set接口的一个具体类，用来存储一个元素之间互不相同的集合,内部基于HashMap实现，对于插入的元素，不维持元素的有序性
* LinkedHashSet: 
* TreeSet: 

## List ##

## Queue ##

## Map ##
* HashMap: 
* LinkedHashMap: 
* TreeMap:

# 二、源码分析 #

## HashMap ##

### 1、数据结构 ###
&nbsp;HashMap类内部有一个数组，插入HashMap的键/值对就存储在这个数组中
从内部代码不难看出，HashMap的键值对是由一个数组存储，而数组的每一项又是一个链表的结构  
_HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体_
```
transient Node<K,V>[] table; 
```




## HashSet ##

> This is the first level of quoting.
>
>  This is nested blockquote.
>
> Back to the first level.

