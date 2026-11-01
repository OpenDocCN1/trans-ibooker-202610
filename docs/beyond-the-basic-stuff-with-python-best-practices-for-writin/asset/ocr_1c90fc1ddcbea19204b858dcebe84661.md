```
index 76b5814..3be49c3 100644
--- a/README.md
+++ b/README.md
@@ -13,7 +13,14 @@ To install with pip, run:
Quickstart Guide
---------------

-TODO - fill this in later
+Here's some xample code demonstrating how this module is used:
+
+    >>> import wizcoin
+    >>> coin = wizcoin.WizCoin(2, 5, 10)
+    >>> str(coin)
+    '2g, 5s, 10k'
+    >>> coin.value()
+    1141

Contribute
----------
```

The output shows that README.md in your working copy has changed from the README.md as it exists in the latest commit of the repo. The lines that begin with a minus sign - have been removed; the lines that begin with a plus sign + have been added.

While reviewing the changes, you’ll also notice that we made a typo by writing xample instead of example. We shouldn’t check in this typo. Let’s correct it. Then run git diff again to inspect the change and add and commit it to the repo:

```
C:\Users\Al\wizcoin>git diff
diff --git a/README.md b/README.md
index 76b5814..3be49c3 100644
--- a/README.md
+++ b/README.md
@@ -13,7 +13,14 @@ To install with pip, run:
Quickstart Guide
---------------

-TODO - fill this in later
+Here's some example code demonstrating how this module is used:
--snip--
C:\Users\Al\wizcoin>git add README.md

C:\Users\Al\wizcoin>git commit -m "Added example code to README.md"
[master 2a4c5b8] Added example code to README.md
1 file changed, 8 insertions(+), 1 deletion(-)
```

The correction is now safely committed to the repo.

# Using git difftool to View Changes with a GUI Application

It’s easier to see changes with a diff program that uses a GUI. On Windows, you can download WinMerge (https://winmerge.org/), a free, open source diff