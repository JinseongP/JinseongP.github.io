# Personal website


## Codespaces

1. Open new codespace
2. Install lvips
   
```
sudo apt update && sudo apt install libvips-tools
```

3. Install gems
   
```
bundle install
```

4. Run jekyll

```
bundle exec jekyll serve
```



**#** **How to push.**


git add .

git commit -m "update info"

git push origin


**#** **Errors**


If github does not update (maybe gh-pages branch does not sync)

https://stackoverflow.com/questions/24713112/why-does-my-github-page-not-update-its-content

1) git push origin --force

2) git push origin :gh-pages

3) git push origin gh-pages
