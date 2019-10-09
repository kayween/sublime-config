# Skynet User Instructions
Follow the instructions carefully, otherwise some of the commands in this guide will not work.

# Log in

## SSH Command

Run this on local.

```bash
ssh -p 2345 <username>@158.106.83.82
```

## No SSH password login

Run this on local.

```bash
ssh-add -k ~/.ssh/id_rsa
```

# Automate login (IMPORTANT!)

## Modify sshconfig

Do this on local.
Create  `~/.ssh/config`

```bash
Host skynet
    HostName 158.106.83.82
    User <username>
    Port 2345
    ForwardAgent yes
    IdentityFile ~/.ssh/id_rsa
```

```bash
ssh skynet
```

# Installing Anaconda

Run this from server

## Anaconda2(python2.7)

```bash
bash /anaconda_install/Anaconda2-5.0.1-Linux-x86_64.sh
```

## Anaconda3(python3.6)

```bash
bash /anaconda_install/Anaconda3-5.0.1-Linux-x86_64.sh
```

# Installing virtualenv Locally (If you want to use it)

This command will install `virtualenv` binary inside your `~/.local` folder.

```bash
pip install --user virtualenv
```

If the binaries can't be found, add the following command to your `.bashrc` or `.bash_profile` variable.

```bash
export PATH=/home/<user_name>/.local/bin:$PATH
```

# Adding Path Variable (IMPORTANT!)

Run this from server

Open an editor of choice and add these to ``.bashrc`` or ``.bash_profile``

```bash
export PATH=/usr/local/cuda/bin:/usr/local/cuda-9.0/bin:/usr/local/cuda-8.0/bin:$PATH
export PATH=/home/<user_name>/[anaconda2 | anaconda3]/bin:$PATH # you can remove this line if you're using virtualenv

export LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/cuda-9.0/lib64:/usr/local/cuda-8.0/lib64:$LD_LIBRARY_PATH
export XDG_RUNTIME_DIR=""

source /home/jin/.slurm_functions.sh
```

For extra skynet commands/scripts, see `extra_commands.md` in the same folder as this file.

# Sending Files

## Send file

Run this on local

```bash
scp <local_file_name> skynet:~
```

## Send Folder

Run this on local

```bash
scp -r <local_folder_name> skynet:~
```

# Using Slurm

For all jobs there is a 3 day limit. It will be killed if it goes past 3 days.

**DO NOT RUN JOBS ON LOGIN/HEAD NODE**

**DO NOT SSH AND RUN JOBS FROM COMPUTE MACHINES, ALWAYS USE SLURM**

The requirements that are enforced are memory and GPU. You will get to use the whole CPU Regardless of the number of CPUs you ask for.

## Using Batch (Preferred Method)

```bash
cp /home/jin/test/test.sh ~
sbatch ~/test.sh
```

This is what `test.sh` looks like

```bash
#!/bin/bash

#SBATCH --mem=20G
#SBATCH --gres=gpu:1

hostname
whoami
echo $CUDA_VISIBLE_DEVICES
sleep 10
python -c "import torch"
echo "DONE"
```

## Using Interactive (Use for testing)

Open up your favorite console virtualization software(tmux, screen) and run

```bash
srun --gres=gpu:1 --mem=20G --pty bash -l
```

# Useful Commands

*squeue*: Check queue information

```bash
squeue
squeue -u <user_name>
```

*scancel*: Cancel jobs

```bash
scancel <job_id>
scancel -u <user_name>
```

*scontrol*: View current state

```bash
# Get General Job Usage
scontrol show jobid -dd <jobid>
```

*sstat*: View status info

```bash
sstat -j <jobid> --allsteps

# Check Max Memory Usage
sacct --jobs=<jobid> --format=User,JobID,account,Timelimit,elapsed,ReqMem,MaxRss,ExitCode
```

*sinfo*: Check what the nodes are doing

```bash
sinfo
```

*sacct*: Get Accounting data

```bash
sacct
```

*sshare*: Get your fairshare usage statistics

```bash
sshare -a
```

*slurm_usage*: Get GPU and Memory Usage across cluster and individual assignments

```bash
# For GPU and Memory usage across Cluster
slurm_usage

# For GPU, Memory Usage, and GPU assignment per node
slurm_usage <server_name>
```

*gpuview*: Get GPU assignment table for the server

```bash
gpuview <server_name>
```

*gpuid*: Get GPU assignment of the user across cluster

```bash
gpuid               # For yourself
gpuid <user_name>   # For others
```

# Using Your Favourite Editor

## Mount Skynet to Local

Create a folder on your local drive. Instructions for installing [SSHFS](https://github.com/osxfuse/osxfuse/wiki/SSHFS) on Mac. And run the following command.

on `local`
```bash
sshfs skynet:/home/<user_name> <local_folder> -o reconnect,ServerAliveInterval=15,ServerAliveCountMax=3
```

Access files from your local folder using your favourite editor. If you don't have one yet, I recommend [visual studio code](https://code.visualstudio.com/).

## Unmount

You might need to unmount the drive.

on `local`
```bash
umount <local_folder>
```

# Using Jupyter Notebook

## Step By Step

on `interactive session`
**choose a unique port number, if it says address already in use try another**

```bash
jupyter notebook --no-browser --port=<jupyter_port>
```
### SSH with -J option
on `local`
```bash
ssh -A -N -f -L localhost:<jupyter_port>:localhost:<jupyter_port> -J skynet <skynet_username>@<computeXXX>
```

If your local computer username is the same as the skynet username the following command works
```bash
ssh -A -N -f -L localhost:<jupyter_port>:localhost:<jupyter_port> -J skynet <computeXXX>
```

### SSH without -J option
Older versions of ssh might not have implemented the J option, in that case use the following command

```bash
ssh -A -N -f -o "ProxyCommand ssh -W %h:%p skynet" -L localhost:<jupyter_port>:localhost:<jupyter_port> <skynet_username>@<computeXXX>
```

If your local computer username is the same as the skynet username the following command works
```bash
ssh -A -N -f -o "ProxyCommand ssh -W %h:%p skynet" -L localhost:<jupyter_port>:localhost:<jupyter_port> <computeXXX>
```

Go on your favourite browser
```
http://localhost:<jupyter_port>
```

## Script

helpful script `bind_ssh_port.sh`

### New SSH

```bash
#!/bin/bash

username="<skynet_username>"
remote_host=$1
l_port=$2

ssh -A -N -f -L localhost:$l_port:localhost:$l_port -J skynet ${username}@${remote_host}
```

### Old SSH

```bash
#!/bin/bash

username="<skynet_username>"
remote_host=$1
l_port=$2

ssh -A -N -f -o "ProxyCommand ssh -W %h:%p skynet" -L localhost:$l_port:localhost:$l_port ${username}@${remote_host}
```

### Usage

```bash
bash bind_ssh_port.sh <computeXXX> <jupyter_port>
```


# Disclaimers

## Memory Usage

If your job goes over the requested memory, the slurm scheduler will terminate
your job. The actual memory usage is the one that counts, not the virtual
memory. Keep in mind that the poll rate for ``htop``, ``top`` are slower so
whatever it reports may not be correct. If your job forks multiple
processes, all the processes' memory are counted towards the limit.

_*But my job got killed even though it does not use the whole thing!*_

No, The scheduler does not kill jobs when you are using less than requested memory.
This has been extensively tested using the memory tools. Use the sacct command to see
what your MaxRss was and run your jobs accordingly.