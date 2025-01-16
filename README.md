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
       function scrapeLinks() {
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
   ```

## 4. ***Download the Links***
  -  After running the script, a file named `tiktok_video_links.txt` will be downloaded containing the scraped video links.

## Use YT-DLP to Download Videos
  -  After downloading the text file, follow these steps to download the videos using YT-DLP:

1. **Open Your Command Line Interface**:
   - **Windows**: Open Command Prompt. Or Powershell
   - **macOS/Linux**: Open Terminal.

2. **Navigate to the Directory**: Use the `cd` command to navigate to the directory where `yt-dlp.exe` is located. For example:
   ```bash
   cd path\to\your\yt-dlp\directory
   ```

3. ***Run YT-DLP with the Downloaded Links:

  ***Use the following command to download the videos using the links from the text file. Replace "path/to/tiktok_video_links.txt" with the actual path to the downloaded text file:***
   -  ***Windows***:

```cmd
yt-dlp.exe -o "%(uploader)s - %(title).50s - %(id)s.%(ext)s" -a "path\to\tiktok_video_links.txt"
```
  -  ***macOS/Linux***:

```bash
./yt-dlp -o "%(uploader)s - %(title).50s - %(id)s.%(ext)s" -a "path/to/tiktok_video_links.txt"
```
***Explanation of Flags:***

  -  -o: Specifies the output template for naming downloaded videos. In this example:
  -  %(uploader)s: Inserts the uploader's name.
  -  %(title).50s: Trims the video title to 50 characters.
  -  (id)s: Includes the unique video ID.
  -  %(ext)s: Adds the appropriate file extension based on the video format.
  -  -a: Reads video URLs from the specified text file.

***Wait for the Downloads to Complete:***

  -  YT-DLP will start downloading all the videos listed in the text file. Depending on your internet speed and the number of videos, this process may take some time.
Verify the Downloads:

  -  Once the process is complete, check the directory where you ran the command. The videos should be downloaded there, named according to the specified output template.
***(Optional) Customize or Convert Videos***:

***If you want to convert the downloaded videos to another format or adjust their settings, you can use FFmpeg. For example, to convert a video to MP4 format***:
```bash
ffmpeg -i "input_video.ext" -c:v libx264 -c:a aac "output_video.mp4"
```


## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Acknowledgments
- **[YT-DLP](https://github.com/yt-dlp/yt-dlp)** for making video downloading easy and efficient.  
- **[FFmpeg](https://ffmpeg.org)** for providing powerful video processing tools.  
- Thanks to the developers and contributors of open-source software that make tools like this possible.

---

## Disclaimer
This tool is intended for **personal use only**. Ensure compliance with TikTok's Terms of Service, copyright laws, and other applicable regulations when using this scraper. Unauthorized downloading or redistribution of copyrighted content may result in legal consequences. Use this tool responsibly.
