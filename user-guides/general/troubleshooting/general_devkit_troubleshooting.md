# Project Santa Cruz development kit troubleshooting

## General devkit troubleshooting commands

To run these commands, connect to the devkit's Wi-Fi AP (if a Wi-Fi connection has not yet been set up through the [OOBE](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/user-guides/getting_started/oobe.md)), [SSH into the devkit using PuTTY](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/user-guides/general/troubleshooting/ssh_and_serial_connection_setup.md), and enter the commands in the PuTTY terminal.

To redirect any output to a .txt file for further analysis, use the following syntax:

```console
<command> > <file name>.txt
```

For additional information on the Iot Edge commands, please see the [IoT Edge device troubleshooting documentation](https://docs.microsoft.com/en-us/azure/iot-edge/troubleshoot).

|Category:         |Command:                    |Function:                  |
|------------------|----------------------------|---------------------------|
|OS                |cat /etc/os-release         |check Mariner image version |
|OS                |cat /etc/os-subrelease      |check derivative image version |
|OS                |cat /etc/adu-version        |check ADU version |
|Temperature       |cat /sys/class/thermal/thermal_zone0/temp |check temperature of devkit |
|Wi-Fi             |journalctl -u hostapd.service |check SoftAP logs|
|Wi-Fi             |journalctl -u wpa_supplicant.service |check Wi-Fi services logs |
|Wi-Fi             |journalctl -u ztpd.service  |check Wi-Fi Zero Touch Provisioning Service logs |
|Wi-Fi             |journalctl -u systemd-networkd |check Mariner Network stack logs |
|OOBE              |journalctl -u oobe -b       |check OOBE logs |
|Telemetry         |azure-device-health-id      |find unique telemetry HW_ID |
|Iot Edge          |sudo iotedge check          |run configuration and connectivity checks for common issues |
|Iot Edge          |sudo iotedge logs \<container name> |check container logs, such as speech and vision modules |
|Iot Edge          |sudo iotedge support-bundle --since 1h |collect module logs, IoT Edge security manager logs, container engine logs, 'iotedge check' JSON output, and other useful debug information from the past hour |
|Iot Edge          |sudo journalctl -u iotedge -f |view the logs of the IoT Edge security manager |
|Iot Edge          |sudo systemctl restart iotedge |restart the IoT Edge Security Daemon |
|IoT Edge          |sudo iotedge list           |list the deployed iotedge modules |
|Other             |df \<option> \<file>        |display information on available/total space in specified file system(s) |

Note: The Wi-Fi commands can be combined into the following:

```console
journalctl -u hostapd.service -u wpa_supplicant.service -u ztpd.service -u systemd-networkd -b
```

## Docker troubleshooting commands

|Command:                        |Function:                  |
|--------------------------------|---------------------------|
|docker ps                       |shows which containers are running |
|docker images                   |shows which images are on the device |
|docker rmi \<image id> -f       |deletes an image from the device |
|docker logs -f edgeAgent <br> docker logs -f \<module_name> |takes container logs of specified module |
|docker image prune              |removes all dangling images |

## USB Updating

|Error:                                    |Solution:                                               |
|------------------------------------------|--------------------------------------------------------|
|LIBUSB_ERROR_XXX during USB flash via UUU |This error is the result of a USB connection failure during UUU updating. If the USB cable is not properly connected to the USB ports on the PC or the PE-10X, an error of this form will occur. Try unplugging and replugging both ends of the USB cable and jiggling the cable to ensure a secure connection. This almost always solves the issue. |