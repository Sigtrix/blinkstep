# Overview
This embassy based program for the Raspberry Pi Pico W connects to a Wi-Fi network, obtains
an IP address dynamically using DHCP, and listens for step count data over a TCP connection.
The step count data is received over the network, and the system adjusts the behavior of an
LED based on the received step count.

# Building
Place the project in the same top-level directory as a checked out version of 
[embassy](https://github.com/embassy-rs/embassy). Adjust the environment variables in 
`.cargo/config.toml`:

* `WIFI_NETWORK`: Your SSID
* `WIFI_PASSWORD`: Your WPA2 Wi-Fi password

In the project top-level directory `blinkstep` run:

```
cargo build --bin wifi_tcp_server --release
elf2uf2-rs target/thumbv6m-none-eabi/release/wifi_tcp_server wifi_tcp_server.uf2
```

# Installing on the Pico
Copy the `wifi_tcp_server.uf2` file to where your Raspberry Pi Pico W is mounted when booted
with the BOOTSEL button pressed.

# Usage
## Obtaining the Device's IP Address
When the embedded system successfully connects to the Wi-Fi network and obtains an IP
address dynamically using DHCP, it will output the obtained IP address to the serial console.
You can monitor this output using a serial communication tool such as screen or minicom.
Assuming you are using a USB-to-serial adapter and the device appears as
`/dev/cu.usbmodem1234`, you can use the following command:

```
screen /dev/cu.usbmodem1234 115200
```
## Interacting with the Device
Once the device has obtained an IP address, it can be interacted with over the network
using a TCP client. The device listens for incoming connections on port 1234. For example,
you can use the telnet command in the terminal to connect to the device:

```
telnet <device_ip_address> 1234
```
Replace <device_ip_address> with the actual IP address obtained by the device.
This will establish a TCP connection to the device on port 1234.
After establishing a connection, you can send step count data to the device. 
The format should be a string representation of the step count, terminated with
a newline character (\n).

