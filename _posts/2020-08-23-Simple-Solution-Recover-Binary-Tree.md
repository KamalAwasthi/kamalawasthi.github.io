---
title: Simple Solution to Recover Binary Tree
date: 2020-08-23 00:00:00 Z
categories:
- Algorithms
tags:
- STL
- DS
- Trees
- Algo
layout: single
excerpt: This is a simple iterative solution to Recover Binary Tree Problem given
  on Leetcode and GeeksForGeeks
header:
  overlay_color: "#333"
  teaser: "/assets/images/coding.jpg"
last_modified_at: 2020-08-23 00:00:00 Z
---

This is a simple iterative solution using one Stack to Recover Binary Tree Problem given on Leetcode([Link](https://leetcode.com/problems/recover-binary-search-tree/){:target="_blank"}) & GeeksForGeeks([Link](https://www.geeksforgeeks.org/fix-two-swapped-nodes-of-bst/){:target="_blank"})

> Problem Statement: Two elements of a binary search tree (BST) are swapped by mistake. Recover the tree without changing its structure. Category: [Hard](#link){: .btn .btn--danger .btn--small}

We start by checking for root. If root is <code>NULL</code> we return <code>NULL</code>.

```
if(!root)
    return;

```

if root is not <code>NULL</code>. We start reading the tree in Inorder manner but before that we declare some variables we will need to do our work during the traversal.
```
stack<TreeNode*> s;
TreeNode* prev=NULL;
TreeNode* first=NULL;
TreeNode* second=NULL;
TreeNode* t=NULL;
```
\*prev pointer is actually to store the previous node value we encounter before the current node. \*first is to store the first misplaced value in the tree. Similarly, \*second pointer stores the second misplaced value. 

Now we start our loop with inorder traversal. We will use the exact same approach we use for iterative inorder traversal of a binary tree. If you are not familiar with this approach you can look up [here](#link){:target="_blank"}. Have a look at the code and then we will discuss what we are actaully doing there.
```
        while(1){
            while(root){
                s.push(root);
                root=root->left;
            }
            if(s.empty())
                break;
            root=s.top();
            s.pop(); //pop the element
            if(prev==NULL)
                prev=root;
            else{
                if(prev->val>root->val && flag==0){
                    first=prev;
                    t=root;
                    flag=1;
                }
                else{ 
                    if(flag==1 && prev->val>root->val){
                        second=root;
                        break;
                    }
                }
                prev=root;
            }
            root=root->right;
        }

        if(first && second){
            int temp=first->val;
            first->val=second->val;
            second->val=temp;
        }
        else{
            second=t;
            int temp=first->val;
            first->val=second->val;
            second->val=temp;
        }
```

Okay so lets discuss what we actually did above. If you are really familiar with the iterative approach of inorder traversal, you must have noticed that the loop is reading the tree in inorder manner. While going in inorder way, we are first initializing the <code>*prev</code> by <code>*root</code> as the first node we will ever encounter will be root. At that moment <code>*prev</code> will be <code>NULL</code> hence we will just update its value to <code>*root</code> and will not do anything else.

After that, now we have <code>*root</code> in <code>*prev</code>, now we start looking for first value which is mis placed. as the inorder traversal always gives sorted order of values in tree, we know our <code>*first</code> will be the first value we encounter as,
```
prev->val > root->val
``` 
This is because everytime <code>*prev</code> is getting updated to <code>*root</code> (try it with some exaple on paper) till we do not find both misplaced elements.  <code>flag==0</code> signifies that we have not yet encountered the first value hence also not the second value. Now when we will make <code>flag=1</code> which will help us in tracking the second value which is misplaced. As we know that this time when we have to find the second value, it will be found when we encounter:
```
prev->val>root->val
```
This will work because now <code>*prev</code> will have the value which is on left on second value, as second value is misplaced it will obviously be smaller ans should have been less than prev. 


Now After completing the loop, two cases arise.

- First is when both misplaced elements are just adjacent to each other in inorder traversal:
	- In this case, we will not able to capture the second element as it will always be less than its right.
	- For this case, we simply store the root value int an extra variable t and assign t to second if we find the vaalue of <code>*second</code> to be NULL even after completion of while loop. 
- We find both the elemnts in loop only
	-then we simply swap their values.

> Happy Coding.