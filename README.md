# TikTok Video Scraper

A simple JavaScript-based scraper for extracting TikTok video links from a user's profile or video feed. This tool allows you to easily collect video URLs and download them using YT-DLP.

## Features

- Scrapes video links from TikTok pages.
- Exports the links to a text file for easy downloading.
- Compatible with YT-DLP for downloading videos.

## Prerequisites

- A modern web browser (Chrome, Firefox, etc.)
- [YT-DLP](https://github.com/yt-dlp/yt-dlp) installed on your system.
- [FFmpeg](https://ffmpeg.org/download.html) installed on your system (optional, but recommended for video processing).

## Installation

### 1. Download YT-DLP

- Go to the [YT-DLP Releases page](https://github.com/yt-dlp/yt-dlp/releases).
- Download the appropriate executable for your operating system:
  - **Windows**: Download `yt-dlp.exe`.
  - **macOS**: Download `yt-dlp` (you may need to make it executable).
  - **Linux**: Download `yt-dlp` (you may need to make it executable).

### 2. Download FFmpeg

- Go to the [FFmpeg Download page](https://ffmpeg.org/download.html).
- Follow the instructions for your operating system to download and install FFmpeg.
  - **Windows**: You can download a build from [FFmpeg Builds](https://www.gyan.dev/ffmpeg/builds/). Extract the files and add the `bin` folder to your system's PATH.
  - **macOS**: You can install FFmpeg using Homebrew with the command:
    ```bash
    brew install ffmpeg
    ```
  - **Linux**: You can install FFmpeg using your package manager. For example, on Ubuntu:
    ```bash
    sudo apt update
    sudo apt install ffmpeg
    ```

### 3. Set Up Your Environment

- **Windows**:
  1. Create a folder for your tools, e.g., `C:\yt-dlp`.
  2. Place the `yt-dlp.exe` file and the `ffmpeg` folder (containing the `bin` folder) in this directory.
  3. Add the `bin` folder of FFmpeg to your system's PATH:
     - Right-click on "This PC" or "My Computer" and select "Properties."
     - Click on "Advanced system settings."
     - Click on "Environment Variables."
     - Under "System variables," find the `Path` variable and click "Edit."
     - Add the path to the `bin` folder of FFmpeg (e.g., `C:\path\to\ffmpeg\bin`).

- **macOS/Linux**:
  1. Place the `yt-dlp` file in a directory that is in your PATH, or you can create a new directory (e.g., `~/yt-dlp`) and add it to your PATH.
  2. Ensure FFmpeg is installed and accessible from the terminal.

## Usage

1. **Navigate to a TikTok Page**: Open the TikTok page containing the videos you want to scrape (e.g., a user's profile or a specific video feed).

2. **Open the Browser Console**:
   - Press `F12` or `Ctrl + Shift + I` (Windows/Linux) or `Cmd + Option + I` (macOS).
   - Go to the "Console" tab.

3. **Run the Scraper**: Copy and paste the following JavaScript code into the console and press `Enter`:

   ```javascript
   // Function to scrape TikTok video links and download them as a text file
   function scrapeTikTokLinksAndDownload() {
       const videoLinks = [];
       
       // Select all video elements on the page
       const videoElements = document.querySelectorAll('a[href*="/video/"]');

       // Loop through each video element and extract the href attribute
       videoElements.forEach(video => {
           const link = video.href;
           if (link && !videoLinks.includes(link)) {
               videoLinks.push(link);
           }
       });

       // Create a text file from the links
       const linksString = videoLinks.join('\n');
       const blob = new Blob([linksString], { type: 'text/plain' });
       const url = URL.createObjectURL(blob);

       // Create a temporary anchor element to trigger the download
       const a = document.createElement('a');
       a.href = url;
       a.download = 'tiktok_video_links.txt'; // Name of the file to be downloaded
       document.body.appendChild(a);
       a.click(); // Trigger the download
       document.body.removeChild(a); // Clean up

       // Revoke the object URL to free up memory
       URL.revokeObjectURL(url);
   }

   // Run the function
   scrapeTikTokLinksAndDownload();
   ```


4. **Download the Links:** After running the code, a text file named tiktok_video_links.txt will be downloaded to your computer containing the scraped video links.

5. **Download Videos Using YT-DLP:**

  - Open your terminal or command prompt.
  - Navigate to the directory where the tiktok_video_links.txt file is located.
  - Run the following command:

```bash
yt-dlp -a "Full path\tiktok_video_links.txt"
```
This command will read the video links from the text file and download the videos using YT-DLP.

License
This project is licensed under the MIT License. See the LICENSE file for details.

Disclaimer
This tool is intended for personal use and educational purposes only. Please respect the copyright and terms of service of TikTok and any other platforms you use this tool with.
