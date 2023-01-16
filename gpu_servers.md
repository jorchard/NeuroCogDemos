# GPU Servers

We have a few GPU machines that we can use.
1. `honeydew.cs.uwaterloo.ca` has an NVIDIA Titan XP with 12GB RAM. We gratefully acknowledge the support of NVIDIA Corporation with the donation of the Titan Xp GPU.
1. `beaker.cs.uwaterloo.ca` has an NVIDIA Titan XP with 12GB RAM. This machine was donated by Morgan McGuire of NVIDIA.
3. `koios.cs.uwaterloo.ca` has 8 NVIDIA Titan XPs, each with 12GB RAM. This machine belongs to Tyler, but he invites us to use it. Thanks Tyler!


## `jupyterhub` on koios

This is a convenient way to use `koios` without having to mess around with `ssh`. The machine is set up with `jupyterhub`. To request an account, go to
```
koios.cs.uwaterloo.ca/hub/signup
```
An admin will then have to go to `koios.cs.uwaterloo.ca/hub/authorize` to authorize the account. After that, just point your browser to
```
koios.cs.uwaterloo.ca
```
To load your code onto it, we suggest using `git`. Open up a terminal, and `git`-away!

___
#### Public vs. Private repository
You can `git clone "<git repo url>"` in the terminal easily. However, this works well only if your repo is public.
For the private repo, you need to grant a permission.
```bahs
cd .ssh
ssh-keygen
```
- For the simplicity, just enter in response to the comments.
- Then open `id_rsa.pub` and copy the content of the file using `vim`, for example.
- Then, on your cloud server, (github, gitlab, ...) , on your `profile > settings`, 
- On the left, side-bar select `SSH and GPG keys` then `new ssh key` and paste the copied key from the clipboard.
- For example, `[https://github.com/settings/keys](https://github.com/settings/ssh/new)`, 
- Obviously a good description helps to recall `SSH->Device` later.
___

## Running jupyter notebook Remotely

1. Login to remote machine
```
ssh honeydew.cs.uwaterloo.ca
```
If you are doing this from off-campus, you might have to use a [VPN](https://uwaterloo.ca/information-systems-technology/services/virtual-private-network-vpn/about-virtual-private-network-vpn).

2. Instantiate  the machine-learning environment (if you want to use `pyTorch`, etc)
```
source activate ml
```
3. On remote machine, start the notebook on a specified port, and run it in the background. The command `nohup` allows the process to continue even if you close the terminal window or logout of the server. Note that the output will then be directed to the file `nohup.out`.
```
nohup jupyter notebook --no-browser --port=8885 &
```
4. Make note of the URL that the notebook is running on. You might have to look in the file `nohup.out` for this. It'll have a really long random string of characters.
5. Log out of remote machine.
6. Create an `ssh` tunnel to the chosen port
```
ssh -N -f -L localhost:8884:localhost:8885 honeydew.cs.uwaterloo.ca
```
7. In your browser, go to the URL that you noted, but replace 8885 with 8884
```
http://localhost:8884
```
You can close the terminal window, or the browser window, but that won’t affect the 
notebook. You can always repeat steps 6 and 7 to continue where you left off. Amazing, right?!


## GPU Monitoring
Type `nvidia-smi` to see the GPU load.
