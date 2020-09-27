# Multiple loop vs single loop

Do you often find yourself writing code like this

for(;;)

{

while(;){}

}

Usually, we have two pointers here - one pointer gets incremented in main for loop and the second pointer gets incremented in inner while loop. When you come across such pattern, think of "two-pointer technique" and see if you can use single loop (i.e. main for loop) only to restructure this code to be more readable. We all have seen this technique in use in merge part of merge-sort.

This is sample problem:

[https://leetcode.com/problems/assign-cookies/](https://www.blogger.com/blog/post/edit/4438036435031235392/4602686676160668539#)

Check the two functions below which are doing the same thing but in a slightly different way. If we cannout use two pointers, we should consider moving the second loop in a different function. The code with single loop is more readable, robust and easier to debug if things go wrong than using multiple loops.

<script src="https://gist.github.com/vamalhotra/c83addfac8507a4928af48a37e9b19a1.js"></script>