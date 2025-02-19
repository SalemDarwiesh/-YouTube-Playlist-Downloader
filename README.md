# YouTube Playlist Downloader

This Python script downloads all videos from a YouTube playlist using the `yt-dlp` library.  It's a simple and efficient way to archive or save playlists for offline viewing.

## Features

*   **Playlist Download:** Downloads entire YouTube playlists.
*   **Best Quality:** Downloads the best available quality video and audio by default (`best` format).
*   **Filename Formatting:** Saves videos with their titles as filenames.
*   **Easy to Use:**  Requires only the playlist URL and a destination directory.

## Requirements

*   Python 3.x
*   `yt-dlp` library:
    ```bash
    pip install yt-dlp
    ```

## Usage

1.  **Install `yt-dlp`:**

    ```bash
    pip install yt-dlp
    ```

2.  **Modify the Script:**

    *   Open the Python script (e.g., `yt_downloader.py`).
    *   Replace the empty string in `playlist_url = ""` with the actual URL of the YouTube playlist you want to download.  **Important:** Make sure it's the *playlist* URL, which usually includes `&list=`.
    *   Replace the empty string in `download_path = ""` with the path to the directory where you want to save the downloaded videos.  This directory must exist.

    Example (within the script):

    ```python
    playlist_url = "https://www.youtube.com/playlist?list=PLQVvvaa0QuDfKTOs3Keq_kaG2P55YRgv3"  # Example playlist
    download_path = "/home/user/Videos/YouTubePlaylists" # Example path (Linux/macOS)
    # OR
    download_path = "C:\\Users\\YourName\\Videos\\YouTubePlaylists" # Example path (Windows)
    ```

3.  **Run the Script:**

    ```bash
    python yt_downloader.py
    ```

    Replace `yt_downloader.py` with the actual name of your script file.

## Script Explanation

*   **`import yt_dlp`:** Imports the `yt-dlp` library.
*   **`download_playlist(playlist_url, download_path)`:** This function does the main work:
    *   **`ydl_opts`:**  A dictionary containing download options.
        *   **`'format': 'best'`:**  Downloads the best combined video and audio format.  Other options include `'bestvideo'` (best video only) and `'bestaudio'` (best audio only).
        *   **`'outtmpl': f'{download_path}/%(title)s.%(ext)s'`:**  Specifies the output filename template.
            *   `download_path`:  The directory you specified.
            *   `%(title)s`:  The video title.
            *   `%(ext)s`:  The file extension (e.g., mp4, webm).
    *   **`with yt_dlp.YoutubeDL(ydl_opts) as ydl:`:** Creates a `YoutubeDL` object with the specified options.  The `with` statement ensures that resources are properly released.
    *   **`ydl.download([playlist_url])`:**  Starts the download process for the given playlist URL. The URL needs to be in a list.
*   **`if __name__ == "__main__":`:**  This block executes only when the script is run directly (not imported as a module).  It sets the `playlist_url` and `download_path` (which you need to modify) and then calls the `download_playlist` function.

## Advanced Options (Optional)

You can customize the `ydl_opts` dictionary to control the download process further.  Here are some useful options:

*   **Specific Formats:**
    ```python
    'format': '137+140'  # Download 1080p video (format code 137) and audio (format code 140)
    ```
    You can find format codes using `yt-dlp -F <video_url>`.

*   **Subtitles:**
    ```python
    'writesubtitles': True,       # Download subtitles
    'writeautomaticsub': True,    # Download automatic subtitles (if available)
    'subtitleslangs': ['en', 'fr'], # Download subtitles in English and French
    ```

*   **Rate Limiting:**
    ```python
    'ratelimit': '500K'  # Limit download speed to 500 KB/s
    ```

*   **Output Directory per Playlist:**
    ```python
        'outtmpl': f'{download_path}/%(playlist_title)s/%(title)s.%(ext)s',
    ```
   This will create a subdirectory for each playlist, named after the playlist's title.
*   **Downloading a range of videos**
    ```python
    'playliststart': 1,          # Only download videos from index 1
    'playlistend': 10,           # ...to index 10.
    'playlist_items': '1,3,5-7', # Download specific video indices.

    ```

* **Ignoring errors**:
    ```python
        'ignoreerrors': True,             # Continue downloading even if some videos fail
        'skip_download': True,  # skip download if any errors occur
    ```

Refer to the `yt-dlp` documentation for a complete list of options: [https://github.com/yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp)

## Troubleshooting

*   **`ERROR: unable to download video data: [Errno 2] No such file or directory`:**  This often means the `download_path` directory doesn't exist.  Create the directory before running the script.
*   **`ERROR: Playlist '<playlist_id>' not found`:**  Double-check that you have the correct playlist URL.  It should look like `https://www.youtube.com/playlist?list=...`.
*   **`ERROR: <video_id>: YouTube said: Unable to extract video data`:** This can happen if YouTube changes its website structure.  Try updating `yt-dlp`: `pip install --upgrade yt-dlp`.
*  **Slow downloads:**  YouTube might throttle downloads.  You can try the `'ratelimit'` option (see "Advanced Options"), but there's no guarantee it will help.

This README provides a clear and comprehensive guide to using the YouTube playlist downloader script, including installation, usage, a script explanation, advanced options, and troubleshooting tips. It is well-formatted and suitable for a GitHub repository.
