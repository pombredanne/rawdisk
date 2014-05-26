RawDisk
=======

A Python library for raw access to filesystems. Started it for educational purposes.

Filesystems Supported
---------------------
* NTFS (GPT/MBR)
* HFS+ (GPT)

Download
--------

	$ git clone https://github.com/dariusbakunas/rawdisk.git

Documentation and usage
-----------------------

```python
from rawdisk import Reader

r = Reader()
r.load('sample_images/ntfs.vhd')
```

* List partitions:

```python
r.list_partitions()
```

```console
Type: NTFS, Offset: 0x10000, Size: 1020M, MFT Table Offset: 0x15465000
```

* Hexdump contents of selected system MFT entry and then its attributes

```python
ntfs_vol = r.partitions[0]

# $MFT index is 0

mft_entry = ntfs_vol.mft_table.get_system_entry(0)

mft_entry.hexdump()

for attr in mft_entry.attributes:
	print
	attr.hexdump()
```

```console
00000000: 46 49 4C 45 30 00 03 00  EA 22 20 00 00 00 00 00  FILE0...." .....
00000010: 01 00 01 00 38 00 01 00  A0 01 00 00 00 04 00 00  ....8...........
00000020: 00 00 00 00 00 00 00 00  06 00 00 00 00 00 00 00  ................
00000030: 02 00 67 88 00 00 00 00  10 00 00 00 60 00 00 00  ..g.........`...
00000040: 00 00 18 00 00 00 00 00  48 00 00 00 18 00 00 00  ........H.......
00000050: 6C B1 67 D5 90 6C CF 01  6C B1 67 D5 90 6C CF 01  l.g..l..l.g..l..
00000060: 6C B1 67 D5 90 6C CF 01  6C B1 67 D5 90 6C CF 01  l.g..l..l.g..l..
00000070: 06 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................
00000080: 00 00 00 00 00 01 00 00  00 00 00 00 00 00 00 00  ................
00000090: 00 00 00 00 00 00 00 00  30 00 00 00 68 00 00 00  ........0...h...
000000A0: 00 00 18 00 00 00 03 00  4A 00 00 00 18 00 01 00  ........J.......
000000B0: 05 00 00 00 00 00 05 00  6C B1 67 D5 90 6C CF 01  ........l.g..l..
000000C0: 6C B1 67 D5 90 6C CF 01  6C B1 67 D5 90 6C CF 01  l.g..l..l.g..l..
000000D0: 6C B1 67 D5 90 6C CF 01  00 40 00 00 00 00 00 00  l.g..l...@......

<...>

00000000: 10 00 00 00 60 00 00 00  00 00 18 00 00 00 00 00  ....`...........
00000010: 48 00 00 00 18 00 00 00  6C B1 67 D5 90 6C CF 01  H.......l.g..l..
00000020: 6C B1 67 D5 90 6C CF 01  6C B1 67 D5 90 6C CF 01  l.g..l..l.g..l..
00000030: 6C B1 67 D5 90 6C CF 01  06 00 00 00 00 00 00 00  l.g..l..........
00000040: 00 00 00 00 00 00 00 00  00 00 00 00 00 01 00 00  ................
00000050: 00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  ................

00000000: 30 00 00 00 68 00 00 00  00 00 18 00 00 00 03 00  0...h...........
00000010: 4A 00 00 00 18 00 01 00  05 00 00 00 00 00 05 00  J...............
00000020: 6C B1 67 D5 90 6C CF 01  6C B1 67 D5 90 6C CF 01  l.g..l..l.g..l..
00000030: 6C B1 67 D5 90 6C CF 01  6C B1 67 D5 90 6C CF 01  l.g..l..l.g..l..
00000040: 00 40 00 00 00 00 00 00  00 40 00 00 00 00 00 00  .@.......@......
00000050: 06 00 00 00 00 00 00 00  04 03 24 00 4D 00 46 00  ..........$.M.F.
00000060: 54 00 00 00 00 00 00 00                           T.......

<...>
```

* List mft entry attributes:
```python
for attr in mft_entry.attributes:
	print attr
```

```console
Type: $STANDARD_INFORMATION Name: N/A Resident Size: 96
Type: $FILE_NAME Name: N/A Resident Size: 104
Type: $DATA Name: N/A Non-Resident Size: 72
Type: $BITMAP Name: N/A Non-Resident Size: 80
```

* Console utility (so far just prints general information)

```bash
$ sudo rawdisk -f sample_images/ntfs.vhd
```

```console
Scheme: MBR
Partitions:
Type: NTFS, Offset: 0x10000, Size: 1020M, MFT Table Offset: 0x15465000
```

```bash
$ sudo rawdisk -f /dev/disk1
```

```console
Scheme: GPT
Partitions:
Type: NTFS, Offset: 0x100000, Size: 14G, MFT Table Offset: 0xC0100000
```

```bash
$ sudo rawdisk -f /dev/disk0
```

```console
Scheme: GPT
Partitions:
Type: EFI System, Offset: 0x5000
Type: HFS+, Offset: 0xC805000
Type: Apple_Boot, Offset: 0xAE820F5000
```

Installation
------------

To install RawDisk, simply run:

	$ python setup.py install

For system-wide installation, you may need to prefix the previous command with ``sudo``:

	$ sudo python setup.py install