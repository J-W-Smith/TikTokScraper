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
     - In the "System variables" section, find the "Path" variable and click "Edit."
     - Click "New" and add the path to the `bin` folder of FFmpeg (e.g., `C:\yt-dlp\ffmpeg\bin`).
     - Click "OK" to close all dialog boxes.

- **macOS/Linux**:
  1. Open a terminal.
  2. Create a directory for your tools, e.g., `~/yt-dlp`.
  3. Move the `yt-dlp` executable and the FFmpeg binary to this directory.
  4. Add the directory to your PATH by editing your shell configuration file (e.g., `.bashrc`, `.zshrc`):
     ```bash
     export PATH="$PATH:~/yt-dlp"
     ```
  5. Save the file and run `source ~/.bashrc` or `source ~/.zshrc` to apply the changes.

## Usage

1. **Navigate to a TikTok Page**: Open the TikTok page containing the videos you want to scrape (e.g., a user's profile or a specific video feed).

2. **Open the Browser Console**:
   - Press `F12` or `Ctrl + Shift + I` (Windows/Linux) or `Cmd + Option + I` (macOS).
   - Go to the "Console" tab.

3. **Run the Scraper**: Copy and paste the following JavaScript code into the console and press `Enter`:

   ```javascript
   // Function to scrape TikTok video links and download them as a text file
   async function scrapeTikTokLinksAndDownload() {
       const videoLinks = new Set(); // Use a Set to avoid duplicates

       // Function to scroll down the page
       function scrollDown() {
           return new Promise((resolve) => {
               window.scrollBy(0, window.innerHeight); // Scroll down by one viewport height
               setTimeout(resolve, 2000); // Wait for 2 seconds for new content to load
           });
       }

       // Function to scrape video links
       function scrapeLinks() ```javascript
           const videoElements = document.querySelectorAll('a[href*="/video/"]');
           videoElements.forEach(video => {
               const link = video.href;
               if (link) {
                   videoLinks.add(link); // Add link to the Set
               }
           });
       }

       let previousCount = 0; // To track the number of links before scrolling
       let currentCount = 0; // To track the number of links after scrolling

       // Keep scrolling until no new videos are loaded
       do {
           previousCount = videoLinks.size; // Store the current count of unique links
           await scrollDown(); // Scroll down and wait for new content
           scrapeLinks(); // Scrape the links after scrolling
           currentCount = videoLinks.size; // Update the current count of unique links
       } while (currentCount > previousCount); // Continue if new links were found

       // Create a text file from the links
       const linksString = Array.from(videoLinks).join('\n');
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





### 4. **Download the Links**: 
- After running the script, a text file named tiktok_video_links.txt will be downloaded to your computer containing all the scraped video links.

### **Notes**
- Ensure that you have permission to scrape content from TikTok and comply with their terms of service.
- The script may take some time to run depending on the number of videos on the page and your internet speed.

### **License**
This project is licensed under the MIT License - see the LICENSE file for details.
