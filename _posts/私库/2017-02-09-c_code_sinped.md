---
title: 高品位代码集
layout: post
author: WenfengShi
category: 私库
tags: [c/c++]
---
博客链接: [http://codeshold.me/2017/02/c_code_sinped.html](http://codeshold.me/2017/02/c_code_sinped.html)

> 编程的品味也有高低之分

1. 单链表元素删除操作 [参考](http://www.changhai.org/articles/miscellaneous/blog/201609.php)

- 低品位

        remove_list_entry(entry)
        {
            prev = NULL;
            walk = entry;

            // Walk the list
            while(walk != entry) {
                prev = walk;
                walk = walk->next;
            }

            // Removing the entry by updating the previous or the head entry
            if(prev) {
                prev->next = entry->next;
            } else {
                head = entry->next;
            }
        }

- 高品位

        remove_list_entry(entry)
        {
            // The "indirect" pointer points to the ADDRESS of the thing we'll update
            indirect = &head;

            // Walk the list, looking for the thing points to the entry we want to remove
            while(*indirect != entry) {
                indirect = &(*indirect)->next;
            }

            // Remove the entry...
            *indirect = entry->next;
        }
