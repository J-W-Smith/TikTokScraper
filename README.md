# TikTok Video Scraper

A simple JavaScript-based scraper for extracting TikTok video links from a user's profile or video feed. This tool allows you to easily collect video URLs and download them using YT-DLP.

## Features

- Scrapes video links from TikTok pages.
- Exports the links to a text file for easy downloading.
- Compatible with YT-DLP for downloading videos.

## Prerequisites

- A modern web browser (Chrome, Firefox, etc.)
- [YT-DLP](https://github.com/yt-dlp/yt-dlp) installed on your system.

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
