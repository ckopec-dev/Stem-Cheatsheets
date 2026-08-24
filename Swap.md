# Swap space

~~~bash
# Show current swap space
sudo swapon --show

# Show current memory usage
free -h

# Allocate a swapfile and set permissions
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile

# Format and activate it
sudo mkswap /swapfile
sudo swapon /swapfile

# Modify fstab to make it persist
sudo nano /etc/fstab
# Add at bottom:
/swapfile none swap sw 0 0

# Sanity check before reboot
sudo findmnt --verify --verbose

# Reboot
sudo shutdown -r now

# Verify changes
sudo swapon --show
free -h
~~~
