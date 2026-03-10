# Introduction to the CLI

## How to connect to a Cisco device?
* Use a rollover cable where one end is a RJ-45 connector and the other is a DB-9 connector
* These are connected in the CONSOLE parts of the device

## Rollover Cable
Insert image

## Terminal Emulator (PuTTy)
* Connection type = Serial
* Category > Connection > Serial 
  Speed (baud) = 9600 
  Data bits = 8 | Stop bits = 1
  (For every 8 data bits sent, one stop bit is sent to mark the end of the byte)
  Parity = None (used to detect errors)
  Flow Control = None (controlling flow of data from transmitter to receiver)

## CLI
Format: 
device_name(privilege_symbol)

#### User EXEC mode (aka user mode) (>) - default, cant do much
#### Privileged EXEC mode (#) - upgraded, can do much
Router> enable
Router# (now im in privileged mode)

#### Global Configuration Mode:
Router> enable
Router# configure terminal
Router (config)# (now im in global config mode)

##### Setting password for privileged EXEC mode:
Router(config)# enable password thisIsThePassword

### Configuration Files
* Running config: The current, active configuration file on the device. As you enter commands in the CLI, you edit the active configuration. 
* Startup config: The configuration file that will be loaded upon restart of the device. 

Commands: (in Privilege EXEC mode)
show running-config
show startup-config

You save the running-config which becomes the startup-configuration:
1. write *or*
2. write memory *or*
3. copy running-config startup-config

When you show the config files, a history of commands is shown, where the *enable password thisISThePassword* command shows the password in plain text --> Security Risk, therefore Encrypt

#### Encrypting Password
1. conf t
2. service password-encryption 
Now, password encrypted w/ Cisco's proprietary algorithm, a number appears before the encrypted password - that indicates the S.No. of the encryption technique used.

#### Tougher secret encryption:
You can set a secret instead of a password and encrypt it with tougher security.
1. enable secret thisIsTheSecret
2. do show running-config (do cmd allows to run other mode commands from global config mode)
Here as well, a number appears before the encrypted password -> 5 indicates MD5 encrpytion.

### Cancelling a command
no "command"
