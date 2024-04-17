# Self-Hosting Instructions for IPFS and IPNS

#### Add your site <a href="#linux" id="linux"></a>

The next step is to import your site into IPFS using the IPFS desktop app you just installed. \
\
The website we'll be using is incredibly simple. The purpose of it is to display a simple clock as a webpage. \
\
This code will display a simple clock that updates every second, showing the current time in hours, minutes, and seconds.

1.  Create a file called `index.html` and paste in the following code:\


    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Simple Clock</title>
        <style>
            body {
                font-family: Arial, sans-serif;
                background-color: #f0f0f0;
                display: flex;
                justify-content: center;
                align-items: center;
                height: 100vh;
                margin: 0;
            }
            #clock {
                font-size: 48px;
            }
        </style>
    </head>
    <body>
        <div id="clock"></div>
        <script>
            function updateClock() {
                const now = new Date();
                const hours = now.getHours().toString().padStart(2, '0');
                const minutes = now.getMinutes().toString().padStart(2, '0');
                const seconds = now.getSeconds().toString().padStart(2, '0');
                document.getElementById('clock').innerText = `${hours}:${minutes}:${seconds}`;
            }

            setInterval(updateClock, 1000);
            updateClock(); // Initial call to display the clock immediately
        </script>
    </body>
    </html>
    ```
2. Open IPFS desktop and go to the **Files** page.
3. Click **Add** → **File**.
4.  Navigate to your `index.html` file and select **Open**.

    ![Choose a file window open in IPFS desktop.](https://docs.ipfs.tech/assets/img/add-ipfs-desktop-open-file.ae222cb9.png)
5. Click the triple dot menu on `index.html` and select **Share link**.
6.  Click **Copy** to copy the file's URL to your clipboard.

    ![Share files window in IPFS desktop.](https://docs.ipfs.tech/assets/img/add-ipfs-desktop-share-files.d89b6b1d.png)
7. Open a browser and paste in the URL you just copied.

Your browser should load the website in a few moments! This can take up to a few minutes the first time. You can move onto the next section while the site is loading.

### [#](https://docs.ipfs.tech/how-to/websites-on-ipfs/single-page-website/#pinning-files)Pinning files <a href="#pinning-files" id="pinning-files"></a>

IPFS nodes treat the data they store like a cache, meaning that there is no guarantee the data will continue to be stored. _Pinning_ a file tells an IPFS node to treat the data as essential and not throw it away. You should _pin_ any content you consider important to ensure that data is retained over the long term. IPFS Desktop allows you to pin files straight from the _Files_ tab.

![IPFS Desktop application showing the pinning option.](https://docs.ipfs.tech/assets/img/ipfs-desktop-showing-pinning.ec877ccb.png)

However, if you want your IPFS data to remain accessible when your local IPFS node goes offline, you might want to use another option like a _pinning service_.

