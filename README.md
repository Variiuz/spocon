# Spotify Connect for Ubuntu, Debian and Raspbian (SpoCon)

## Introduction

SpoCon is a [Spotify Connect](https://www.spotify.com/connect/) client for
[Raspbian](https://www.raspberrypi.org/downloads/raspbian/). SpoCon is a
[Debian package and associated repository](https://en.wikipedia.org/wiki/Deb_\(file_format\))
which thinly wraps the awesome
[librespot-java](https://github.com/librespot-org/librespot-java) library by
[Luca Altomanigian](https://github.com/devgianlu) and others. It works out of the box on
all three revisions of the Pi, immediately after installation.

## Download Latest Version

### SpoCon releases for Ubuntu and Debian on [Launchpad](https://launchpad.net/~spocon/+archive/ubuntu/spocon)
```
sudo add-apt-repository ppa:spocon/spocon
sudo apt-get -y update
sudo apt-get install spocon 
```

### Raspbian releases 

```
sudo apt-get install -y software-properties-common
sudo add-apt-repository -r universe ppa:spocon/spocon
sudo apt-get update
sudo apt-get install spocon
```

### Requirements

You'll need a [Spotify Premium](https://www.spotify.com/premium/) account in order
to use Connect.

### Uninstalling

To uninstall, remove the package,

```bash
sudo apt-get remove -y spocon
```

To completely remove spocon and its Debian repository from your system try,
```bash
sudo apt-get remove -y --purge spocon
```

## Configuration


SpoCon should work out of the box and should be discoverable by Spotify Connect on
your local network, however you can configure it by editing `/opt/spocon/conf.properties`
which passes arguments to [librespot-java](https://github.com/librespot-org/librespot-java).

```
### Device name ###
deviceName=librespot-java
### Device type (Computer, Tablet, Smartphone, Speaker, TV, AVR, STB, AudioDongle, Unknown) ###
deviceType=Computer
### Authentication ###
# Strategy (USER_PASS, ZEROCONF, BLOB, FACEBOOK)
auth.strategy=ZEROCONF
# Spotify username (BLOB, USER_PASS only)
auth.username=
# Spotify password (USER_PASS only)
auth.password=
# Spotify authentication blob (BLOB only)
auth.blob=
### Zeroconf ###
# Listen on this port (`-1` for random)
zeroconf.listenPort=-1
# Listen on all interfaces (overrides `zeroconf.interfaces`)
zeroconf.listenAll=true
# Listen on these interfaces (comma separated list of names)
zeroconf.interfaces=
### Cache ###
# Cache enabled
cache.enabled=true
### Preload ###
# Preload enabled
preload.enabled=true
### Player ###
# Autoplay similar songs when your music ends
player.autoplayEnabled=true
# Preferred audio quality (VORBIS_96, VORBIS_160, VORBIS_320)
player.preferredAudioQuality=VORBIS_160
# Normalisation pregain
player.normalisationPregain=0
# Initial volume (0-65536)
player.initialVolume=65536
# Log available mixers
player.logAvailableMixers=true
# Mixer/backend search keywords
player.mixerSearchKeywords=
# Use CDN to download tracks
player.tracks.useCdn=true
# Use CDN to download episodes
player.episodes.useCdn=true
# Enable loading state (useful for slow connections)
player.enableLoadingState=true
```

After editing restart the daemon by running: `sudo systemctl restart spocon`

## Building the Package Yourself

### Requirements

- Vagrant installed ( https://www.vagrantup.com/ )
- Virtualbox installed ( https://www.virtualbox.org/wiki/Downloads )


```bash
cd Vagrant
vagrant up # start your environment
vagrant ssh # login into you environment
cd workspace
ansible-playbook Ansible/start.yml -e librespot_version=0.5.2 -e spocon_version=0.14.0
```

There should be a built Debian package (a `.deb` file) in your project directory /package.


## Troubleshooting

> *My volume on Spotify is 100% and it's still too quiet!*

Have you tried turning the volume up using the command `alsamixer`?

> *My Raspberry Pi does not use my USB sound card!*

Try to replace the following in the file `/usr/share/alsa/alsa.conf`:

```
defaults.ctl.card 0
defaults.pcm.card 0
```
with
```
defaults.ctl.card 1
defaults.pcm.card 1
```
> *The audio output buzzes a few seconds after audio stops!*

This is likely to be ALSA's Dynamic Audio Power Management (DAPM) shutting down
the sound module of your device to save power. If you want to disable this feature,
create a file called `snd_soc_core.conf` in `/etc/modprobe.d` with this line in:
```
options snd_soc_core pmdown_time -1
```
Once you reboot and play some sound, the issue should be gone.

> *Other issues*

File an issue and if we get it sorted, I'll add to this list.

## Donations

(I'd rather you donate to [librespot-java](https://github.com/librespot-org/librespot-java)
instead, but there's no public address for those folks.)

## Acknowledgments

Special thanks to [Paul Lietar](https://github.com/plietar) for
[librespot](https://github.com/librespot-org/librespot) (and its additional authors) and [David Cooper](https://github.com/dtcooper)


