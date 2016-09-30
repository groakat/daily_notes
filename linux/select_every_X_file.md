# Select every X file in a folder

Use `ls | awk 'NR % X == 0'`, i.e. if `X` is 1000, use `ls | awk 'NR % 1000 == 0'`

# Sync every X file in a folder

First write out the file paths into a text file and then use `rsync`


```bash
IN_PATH='/media/scratch/image_folder/'
ls $IN_PATH | awk 'NR % 100 == 0' > files.txt
rsync -avz -e "ssh -i faster-rcnn.pem" --files-from=files.txt  $IN_PATH ubuntu@52.212.21.43:/home/ubuntu/Documents/data/
```
