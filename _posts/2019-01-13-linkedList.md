---
layout: posts
title: LinkedList
date: 2019-01-13 20:56:00 +0900
type: posts
published: true
comments: true
categories: [datastructure]
tags: [algorithm, datastructure]
---

```
#include <stdio.h>
#include <stdlib.h>

typedef struct _NODE {
	char Data;
	struct _NODE *Next;
} NODE;
typedef struct _DNODE {
	char Data;
	struct _DNODE *Next;
	struct _DNODE *Prev;
} DNODE;

NODE *head, *end, *temp;
NODE *temp1, *temp2, *temp3, *temp4;

void Initialize(void);
void InsertNode(NODE *);
void DeleteNode(NODE *);

void Initialize(void) {
	NODE *ptr;
	head = (NODE*)malloc(sizeof(NODE));
	end = (NODE*)malloc(sizeof(NODE));

	//추가
	temp1 = (NODE*)malloc(sizeof(NODE));
	temp1->Data = 'A';
	head->Next = temp1;
	temp1->Next = end;
	ptr = temp1;

	temp2 = (NODE*)malloc(sizeof(NODE));
	temp2->Data = 'B';
	ptr->Next = temp2;
	temp2->Next = end;
	ptr = temp2;

	temp3 = (NODE*)malloc(sizeof(NODE));
	temp3->Data = 'D';
	ptr->Next = temp3;
	temp3->Next = end;
	ptr = temp3;

	temp4 = (NODE*)malloc(sizeof(NODE));
	temp4->Data = 'E';
	ptr->Next = temp4;
	temp4->Next = end;
	ptr = temp4;
}

void InsertNode(NODE *ptr) {
	NODE *indexptr;
	//순회
	for (indexptr = head; indexptr != end; indexptr = indexptr->Next) {
		//if next data is bigger than ptr data
		if (indexptr->Next->Data > ptr->Data) break;
	}
	ptr->Next = indexptr->Next;
	indexptr->Next = ptr;
}
void DeleteNode(NODE *ptr) {
	NODE *indexptr;
	NODE *deleteptr;

	for (indexptr = head; indexptr != end; indexptr = indexptr->Next) {
		if (indexptr->Next->Data == ptr->Data) {
			deleteptr = indexptr->Next;
			break;
		}
	}
	indexptr->Next = indexptr->Next->Next;
	free(deleteptr);
}

void main() {
	NODE *ptr;
	int i = 0;
	Initialize();

	//print current list
	ptr = head->Next;
	for (i = 0; i < 4; i++) {
		printf("%2c", ptr->Data);
		ptr = ptr->Next;
	}

	printf("\n");
	//make new node
	temp = (NODE*)malloc(sizeof(NODE));
	temp->Data = 'C';

	//insert
	InsertNode(temp);

	ptr = head->Next;

	for (i = 0; i < 5; i++) {
		printf("%2c", ptr->Data);
		ptr = ptr->Next;
	}

	//delete
	DeleteNode(temp);

	//print
	print("\n노드 C의 삭제후\n");
	ptr = head->Next;

	for (i = 0; i < 4; i++) {
		printf("%2c", ptr->Data);
		ptr = ptr->Next;
	}
}
```
