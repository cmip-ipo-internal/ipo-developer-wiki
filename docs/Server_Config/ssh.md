# SSH login and RSA keys

!!! info VM Considerations

    We need to ensure that port 22 is allowed in the security groups before we can connect to the machine through ssh. 
    
    Similarly we also need to assign it a Floating IP such that the machine is globally accessible. 


## Creating a new ssh keygen 
We can create a new RSA pair using the following command. 

```bash 
ssh-keygen -t rsa -C "yourname@work_email.ext"
```

## Additional considerations

### Adding a key to a local agent
We can add the key to our local agent to use keyless login. This is done through 
```bash 
ssh-add /Absolute/Path/to/.ssh/keyname
```

### Ensure the key has the correct permissions
```bash 
PRIVATE_KEY_PATH="</Absolute/Path/to/.ssh/keyname>"
chmod 700 ~/.ssh
chmod 600 $PRIVATE_KEY_PATH

```


## Remote Machine 
### Logging in: 
This requires the use of a key already in the machines 'approved hosts'.
```bash
REMOTE_HOST="<user>@<ip.address>"
PRIVATE_KEY_PATH="</Absolute/Path/to/.ssh/keyname>"

ssh $REMOTE_HOST -i $PRIVATE_KEY_PATH -v

```

### Copy the public key to the remote host
To add a new key to a machine through which we already have access to, we can use: 

```bash 

PRIVATE_KEY_PATH="</Absolute/Path/to/.ssh/keyname>"
REMOTE_HOST="<user>@<ip.address>"
REMOTE_PORT=22

ssh-copy-id -i "$PRIVATE_KEY_PATH.pub" -p $REMOTE_PORT $REMOTE_HOST


```

### Alternative
It is also possible to fetch the host key and append it to the known_hosts file
```bash
ssh-keyscan -t rsa -p $REMOTE_PORT $REMOTE_HOST >> ~/.ssh/known_hosts

```
