# Rename files into sequence order

http://stackoverflow.com/questions/3211595/renaming-files-in-a-folder-to-sequential-numbers
```bash
a=1
for i in *.png; do
  new=$(printf "image-%05d.png" "$a") #04 pad to length of 4
  mv -- "$i" "$new"
  let a=a+1
done
```
