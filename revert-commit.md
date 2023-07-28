## _Step 1_
Resets index to former commit; replace '56e05fced' with your commit code
```sh
 git reset 56e05fced 
 ```

## _Step 2_
Moves pointer back to previous HEAD 
```sh
git reset --soft HEAD@{1} 
git commit -m "Revert to 56e05fced" 
 ```

## _Step 3_
Updates working copy to reflect the new commit 
```sh
git reset --hard 
 ```

## _Step 4_
Push your changes to respective branch 
```sh
git push -f
 ```
