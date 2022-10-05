
## Steps to mount Google Photos as local drive

**Create remote**  
1. rclone config => Refer to [rclone manual](https://rclone.org/googlephotos/)
2. But in the step **Use auto config?** => type **n**  
  The reason is because I was not able to open browser to retrieve authorization code in the docker, choose not to auto config will let you authorize rclone in your host.
3. Copy the codes and paste to the console.

**List photos**  
1. `rclone lsd gdrive:` Show the structure in the top directory.  
2. Or `rclone lsd gdrive:album` to get all albums in the remote.  
3. `rclone ls gdrive:album/<name>` will list all files in that album.  
4. `rclone copy gdrive:album/<name> <path to local drive>` can download photos to local drive.  