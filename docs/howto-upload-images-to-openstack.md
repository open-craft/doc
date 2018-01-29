How to upload images to Openstack
=================================

Preparatory steps
-----------------

1. Install the openstack client, e.g. by running `pip install python-openstackclient` in a fresh virtualenv.
2. Install gpg, and download [signature keys for ubuntu](https://wiki.edubuntu.org/SecurityTeam/FAQ) When I was 
   downloading the images following fingerprint was used: `D2EB 4462 6FDD C30B 513D 5BB7 1A5D 6C4C 7DB8 7C81`, 
   with following short ID: `7DB87C81`
3. Download signing key: `gpg --keyserver keyserver.ubuntu.com --recv-keys 7DB87C81`
 
Download the images
-------------------
 
1. Download the images from: https://cloud-images.ubuntu.com/. Example file to download: 
   https://cloud-images.ubuntu.com/trusty/current/trusty-server-cloudimg-amd64-disk1.img
2. Download `SHA256SUMS.txt` and `SHA256SUMS.gpg`, verify these files `gpg --verify SHA256SUMS.gpg SHA256SUMS.txt`
3. Verify downloaded image: `sha256sum --check SHA256SUMS.txt`

Upload the images
-----------------

1. Download the rc file: go to horizon console `Access ans Security` -> `API Access` -> `Download RC File`.
2. Source openstack rc file. 
3. Upload the image:

        openstack image create --container-format=bare --disk-format=qcow2 \
            --file trusty-server-cloudimg-amd64-disk1.img trusty-14.04-unmodified-`date +%Y%m%d`

   Note image format is qcow2 despite different image extension.  It should be possible to use the `glance`
   client instead of the `openstack` command-line tool, but we had problems with `glance` ignoring the OpenStack
   region in the past.
