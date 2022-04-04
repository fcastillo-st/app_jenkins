By default, oVirt uses spice to display a VMs screen. I've had nothing but problems trying to get spice to work with OSX. VNC is an option, but manually copying the data out of the downloaded .vv files into a VNC viewer before the link times out is even more of a pain. I stumbled across a post online which I found incredibly helpful. It uses OSX's built-in VNC client to connect to VMs which have VNC set as one of the available console types. I'm reposting it here in the hopes that it'll help someone else.

```

vi /usr/local/bin/virt-viewer.py

```


```

chmod +x /usr/local/bin/virt-viewer.py

```

Se recomienda asociar el archivo .vv con una aplicacion del sistema operativo el utilitario automator.

