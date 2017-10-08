# Export Docker images
How to export docker images to another host

## 1. Create a list of images that you want to export

e.g. ``` IDS=$(docker images | awk '{if ($1 ~ /^(centos)/) print $1}')```

Obs: Make sure to get the name of image otherwise the exported image won't have their original name or tag.

## 2. Export the image STDOUT (no output file)

e.g. ``` for x in $IDS;do  docker save $x | ssh username@host 'docker load';done```

In this case, we can use 'pv' in order to monitor the process. 
e.g. ``` for x in $IDS;do  docker save $x | pv | ssh username@host 'docker load';done```

We can also add some compression to this process. Don't forget to decompress the file on the destination host

e.g. ``` for x in $IDS;do  docker save $x | bzip2 | pv | ssh username@host 'bunzip2 | docker load';done```
