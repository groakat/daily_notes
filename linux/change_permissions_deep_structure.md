# Change permissions within deep folder structure

Want to copy more files into a folder. To protect yourself from overwriten the existing ones, do (group only)

    sudo find . -type f -exec chmod g-w {} \;
    
http://stackoverflow.com/a/22083532/2156909
