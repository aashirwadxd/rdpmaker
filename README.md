# how to make rdp
(1) create a  repository 
name anything what you want
<br>
(2) set to private 
<br>
(3) go to settings Actions secrets and variables -->(Actions)
click new secret in name NGROK_AUTH_TOKEN 
in secret you want ngrok auth token 
# NGORK WEBSITE
(1) CREATE ACCOUNT
<br>
(2) WHEN ACCOUNT CREATED 
<br>
(3) CLICK NAVBAR AND CLICK ( Your Authtoken)
<br>
(4)COPY AUTHTOKEN 
# GITHUB
(1) PASTE YOUR AUTH TOKEN IN SECRET AND 
(2) go to action and click set up workflow 
(3) and paste this code
<br>

name: CI
on: [push, workflow_dispatch]
jobs:
  build:
    runs-on: windows-latest
    steps:

    - name: Download

      run: Invoke-WebRequest https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-windows-amd64.zip -OutFile ngrok.zip

    - name: Extract

      run: Expand-Archive ngrok.zip

    - name: Auth

      run: .\ngrok\ngrok.exe authtoken $Env:NGROK_AUTH_TOKEN

      env:

        NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}

    - name: Enable TS

      run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-name "fDenyTSConnections" -Value 0

    - run: Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

    - run: Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1

    - run: Set-LocalUser -Name "runneradmin" -Password (ConvertTo-SecureString -AsPlainText "P@ssw0rd!" -Force)

    - name: Create Tunnel

      run: .\ngrok\ngrok.exe tcp 3389

and click done wait 
go ngrock and click cloud ege and ens point and your ip show
