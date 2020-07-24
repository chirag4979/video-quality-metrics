# Features
- Encodes a video file with every specified preset without having to manually start a new encode with each preset.
- Choose whether you want the libx264 or libx265 encoder to be used.
- Choose the CRF value to be used.
- You can choose whether you want the whole video to be encoded or just a certain amount of seconds.
- The time taken for each preset, the resulting filesize, the filesize compared to the original (as a percentage), the VMAF score and (optionally) the SSIM and/or PSNR of each encode is presented in a table.
# How to use
```
Arguments in square brackets are optional:
python compare-presets.py [-h] -f VIDEO_PATH [-e {libx264,libx265}] [-crf CRF_VALUE] [-t ENCODING_TIME] -p presets
                          [presets ...] [-psnr] [-ssim] [-pm] [-dqs]

If there is a space in the path, it must be surrounded with double quotes. Example:
python compare-presets.py -f "C:/Users/H/Desktop/file 1.mp4" -p veryfast superfast

optional arguments:
  -h, --help            show this help message and exit
  -f VIDEO_PATH, --video-path VIDEO_PATH
                        Enter the path of the video. A relative or absolute path can be specified.If the path contains a space, it must be surrounded in double quotes.
                        Example: -path "C:/Users/H/Desktop/file 1.mp4"
  -e {libx264,libx265}, --video-encoder {libx264,libx265}
                        Specify the encoder to use. Must enter libx264 or libx265. Default: libx264
                        Example: -e libx265
  -crf CRF_VALUE, --crf-value CRF_VALUE
                        Enter the CRF value to be used (default: 23)
  -t ENCODING_TIME, --encoding-time ENCODING_TIME
                        Encode this many seconds of the video. If not specified, the whole video will get encoded.
  -p presets [presets ...], --presets presets [presets ...]
                        List the presets you want to be tested (separated by a space).
                        Choose from: 'veryslow', 'slower', 'slow', 'medium', 'fast', 'faster', 'veryfast', 'superfast', 'ultrafast'
                        Example: -p fast veryfast ultrafast
  -psnr, --calculate-psnr
                        Calculate PSNR in addition to VMAF for each preset.
  -ssim, --calculate-ssim
                        Calculate SSIM in addition to VMAF for each preset.
  -pm, --phone-model    Enable VMAF phone model (default: False)
  -dqs, --disable-quality-stats
                        Disable calculation of PSNR, SSIM and VMAF; only show encoding time and filesize (improves completion time).
```
# Requirements
- Python 3.6+
- FFmpeg installed and in your PATH. The build of FFmpeg must have `--enable-libvmaf` in the configuration (unless you don't care about the quality metrics and you specify `-dqs` when running this command line program). If you're on Windows, you can download an FFmpeg binary which has `--enable-libvmaf` in the configuration by clicking on [this](http://learnffmpeg.s3.amazonaws.com/ffmpeg-vmaf-static-bin.zip) link.
- The files **vmaf_v0.6.1.pkl** and **vmaf_v0.6.1.pkl.model** need to be in the same directory as compare-presets.py
- `pip install -r requirements.txt`
# Example
The table is saved in a .txt file. Here's an example of the table that is produced:
```
You chose to encode a30.mkv using libx264 with a CRF of 23.
+-----------+-------------------+----------+---------------------------+-------+------+-------+
|   Preset  | Encoding Time (s) |   Size   | Size Compared to Original |  PSNR | SSIM |  VMAF |
+-----------+-------------------+----------+---------------------------+-------+------+-------+
|    slow   |       31.65       | 11.58 MB |           71.87%          | 43.81 | 1.0  | 95.55 |
|   medium  |       22.57       | 12.07 MB |           74.93%          | 43.84 | 1.0  | 95.56 |
|    fast   |       18.83       | 12.49 MB |           77.5%           | 43.77 | 1.0  | 95.22 |
|   faster  |       14.58       | 11.79 MB |           73.17%          | 43.59 | 1.0  | 94.86 |
|  veryfast |        9.06       | 9.95 MB  |           61.77%          | 42.28 | 0.99 | 91.99 |
| superfast |        6.51       | 15.64 MB |           97.07%          | 43.46 | 1.0  | 94.43 |
| ultrafast |        4.24       | 31.89 MB |          197.95%          | 42.63 | 0.99 | 96.55 |
+-----------+-------------------+----------+---------------------------+-------+------+-------+
```
*Note: A video file with a length of 30 seconds was encoded.*

*Command: `python compare-presets.py -f a30.mkv -p slow medium fast faster veryfast superfast ultrafast`*
