<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Categorized M3U Web Media Player</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #1a202c; /* Dark background */
            color: #e2e8f0; /* Light text */
            display: flex;
            justify-content: center;
            align-items: flex-start; /* Align to top for better content flow */
            min-height: 100vh;
            padding: 1rem;
            box-sizing: border-box;
        }
        .container {
            width: 100%;
            max-width: 1200px; /* Increased max width for more channels */
            background-color: #2d3748; /* Slightly lighter dark background for container */
            border-radius: 0.75rem; /* Rounded corners */
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05); /* Subtle shadow */
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
        }
        video {
            width: 100%;
            max-height: 70vh; /* Limit video height */
            background-color: #000;
            border-radius: 0.5rem;
            object-fit: contain; /* Ensure video fits within its bounds */
        }
        .message-box {
            background-color: #38a169; /* Green for success */
            color: white;
            padding: 0.75rem 1rem;
            border-radius: 0.5rem;
            margin-top: 1rem;
            display: none; /* Hidden by default */
        }
        .message-box.error {
            background-color: #e53e3e; /* Red for error */
        }
        .message-box.show {
            display: block;
        }
        /* Custom scrollbar for channel list */
        .channel-list-container::-webkit-scrollbar {
            width: 8px;
        }
        .channel-list-container::-webkit-scrollbar-track {
            background: #2d3748;
            border-radius: 10px;
        }
        .channel-list-container::-webkit-scrollbar-thumb {
            background: #4a5568;
            border-radius: 10px;
        }
        .channel-list-container::-webkit-scrollbar-thumb:hover {
            background: #63b3ed;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 class="text-3xl font-bold text-center mb-4">Categorized Web Media Player</h1>

        <video id="videoPlayer" controls autoplay class="rounded-lg shadow-lg"></video>

        <div id="categoryTabs" class="flex flex-wrap gap-2 justify-center mt-4">
            </div>

        <div id="channelList" class="channel-list-container overflow-y-auto max-h-96 p-2 rounded-lg bg-gray-800">
            <div class="text-center text-lg text-gray-400 p-4">Loading channels...</div>
        </div>

        <div id="messageBox" class="message-box"></div>
    </div>

    <script>
        // Global variables for Firebase configuration (not used in this app, but kept for Canvas environment consistency)
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : null;
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        // Predefined list of M3U playlists
        const predefinedPlaylists = [
            { name: "Animation", url: "https://iptv-org.github.io/iptv/categories/animation.m3u" },
            { name: "Classic", url: "https://iptv-org.github.io/iptv/categories/classic.m3u" },
            { name: "Entertainment", url: "https://iptv-org.github.io/iptv/categories/entertainment.m3u" },
            { name: "Kids", url: "https://iptv-org.github.io/iptv/categories/kids.m3u" }
        ];

        let allChannelsData = {}; // Stores channels grouped by category
        let currentCategory = ''; // Tracks the currently selected category
        let currentHls = null; // To manage HLS.js instance

        // Get DOM elements
        const videoPlayer = document.getElementById('videoPlayer');
        const categoryTabs = document.getElementById('categoryTabs');
        const channelList = document.getElementById('channelList');
        const messageBox = document.getElementById('messageBox');

        /**
         * Displays a message in the message box.
         * @param {string} message - The message to display.
         * @param {boolean} isError - True if it's an error message, false otherwise.
         */
        function showMessage(message, isError = false) {
            messageBox.textContent = message;
            messageBox.classList.remove('success', 'error', 'show'); // Reset classes
            messageBox.classList.add('show', isError ? 'error' : 'success');
            setTimeout(() => {
                messageBox.classList.remove('show');
            }, 5000); // Hide after 5 seconds
        }

        /**
         * Parses an M3U playlist content and extracts channel details including logo and group.
         * @param {string} m3uContent - The raw M3U file content.
         * @param {string} defaultCategoryName - The category name to use if group-title is not found.
         * @returns {Array<Object>} An array of channel objects { name, url, logo, category }.
         */
        function parseM3u(m3uContent, defaultCategoryName) {
            const lines = m3uContent.split('\n');
            const channels = [];
            let currentChannel = {};

            for (let i = 0; i < lines.length; i++) {
                const line = lines[i].trim();

                if (line.startsWith('#EXTINF:')) {
                    // Extract channel name from the end of the #EXTINF line
                    const nameMatch = line.match(/,(.+)$/);
                    currentChannel.name = nameMatch && nameMatch[1] ? nameMatch[1].trim() : 'Unknown Channel';

                    // Extract tvg-logo attribute
                    const logoMatch = line.match(/tvg-logo="([^"]+)"/);
                    currentChannel.logo = logoMatch && logoMatch[1] ? logoMatch[1] : '';

                    // Extract group-title attribute
                    const groupMatch = line.match(/group-title="([^"]+)"/);
                    // Use parsed group-title, otherwise fall back to the default category name
                    currentChannel.category = groupMatch && groupMatch[1] ? groupMatch[1] : defaultCategoryName;

                } else if (line.startsWith('http://') || line.startsWith('https://')) {
                    // This line is the URL for the current channel
                    currentChannel.url = line;
                    if (currentChannel.name && currentChannel.url) {
                        channels.push({ ...currentChannel }); // Push a copy of the current channel object
                    }
                    currentChannel = {}; // Reset for the next channel
                }
            }
            return channels;
        }

        /**
         * Fetches and parses all predefined M3U playlists.
         * Populates the allChannelsData object.
         */
        async function fetchAndParseAllPlaylists() {
            showMessage('Loading all playlists and channels...', false);
            categoryTabs.innerHTML = '<div class="text-center text-lg text-gray-400">Loading categories...</div>';
            channelList.innerHTML = '<div class="text-center text-lg text-gray-400">Loading channels...</div>';

            allChannelsData = {}; // Clear previous data

            const fetchPromises = predefinedPlaylists.map(async (playlist) => {
                try {
                    const response = await fetch(playlist.url);
                    if (!response.ok) {
                        throw new Error(`HTTP error! status: ${response.status} for ${playlist.name}`);
                    }
                    const m3uContent = await response.text();
                    // Pass playlist name as default category if group-title is missing in M3U
                    const channels = parseM3u(m3uContent, playlist.name);

                    // Group channels by their parsed category or default category
                    channels.forEach(channel => {
                        const categoryKey = channel.category || playlist.name; // Ensure category key exists
                        if (!allChannelsData[categoryKey]) {
                            allChannelsData[categoryKey] = [];
                        }
                        allChannelsData[categoryKey].push(channel);
                    });

                } catch (error) {
                    console.error(`Error loading or parsing ${playlist.name} playlist:`, error);
                    // Show error but don't stop other playlists from loading
                    showMessage(`Failed to load ${playlist.name} playlist: ${error.message}`, true);
                }
            });

            await Promise.all(fetchPromises); // Wait for all fetches to complete

            if (Object.keys(allChannelsData).length === 0) {
                showMessage('No channels could be loaded from any playlist. Please check URLs.', true);
                categoryTabs.innerHTML = ''; // Clear loading message
                channelList.innerHTML = '<div class="text-center text-lg text-red-400">No channels available.</div>';
            } else {
                showMessage('All playlists loaded successfully!');
                renderCategoryTabs();
                // Set initial category to the first one available and render its channels
                currentCategory = Object.keys(allChannelsData).sort()[0]; // Sort to ensure consistent initial category
                renderChannels();
            }
        }

        /**
         * Renders the category tabs based on the loaded channels.
         */
        function renderCategoryTabs() {
            categoryTabs.innerHTML = ''; // Clear existing tabs
            const categories = Object.keys(allChannelsData).sort(); // Sort categories alphabetically for consistent order

            categories.forEach(category => {
                const button = document.createElement('button');
                button.textContent = category;
                button.classList.add(
                    'px-4', 'py-2', 'rounded-md', 'font-semibold', 'transition-colors', 'duration-200',
                    'mr-2', 'mb-2', // Margin for spacing between buttons
                    'hover:bg-blue-700', 'focus:outline-none', 'focus:ring-2', 'focus:ring-blue-500', 'focus:ring-opacity-75'
                );
                if (category === currentCategory) {
                    button.classList.add('bg-blue-600', 'text-white'); // Active tab styling
                } else {
                    button.classList.add('bg-gray-700', 'text-gray-200'); // Inactive tab styling
                }
                button.addEventListener('click', () => {
                    currentCategory = category;
                    renderCategoryTabs(); // Re-render tabs to update active state
                    renderChannels(); // Render channels for the newly selected category
                });
                categoryTabs.appendChild(button);
            });
        }

        /**
         * Renders channels for the currently selected category in a grid layout.
         */
        function renderChannels() {
            channelList.innerHTML = ''; // Clear existing channels
            if (!currentCategory || !allChannelsData[currentCategory]) {
                channelList.innerHTML = '<div class="text-center text-lg text-gray-400 p-4">Select a category to view channels.</div>';
                return;
            }

            const channels = allChannelsData[currentCategory];
            if (channels.length === 0) {
                channelList.innerHTML = `<div class="text-center text-lg text-gray-400 p-4">No channels found in "${currentCategory}" category.</div>`;
                return;
            }

            const gridContainer = document.createElement('div');
            // Responsive grid columns: 2 on small, 3 on medium, 4 on large, 5 on extra large screens
            gridContainer.classList.add('grid', 'grid-cols-2', 'sm:grid-cols-3', 'md:grid-cols-4', 'lg:grid-cols-5', 'xl:grid-cols-6', 'gap-4', 'mt-4');

            channels.forEach(channel => {
                const channelCard = document.createElement('div');
                channelCard.classList.add(
                    'bg-gray-700', 'p-3', 'rounded-lg', 'shadow-md', 'cursor-pointer',
                    'flex', 'flex-col', 'items-center', 'text-center', 'hover:bg-gray-600',
                    'transition-colors', 'duration-200', 'h-full' // Ensure cards have equal height
                );
                channelCard.title = channel.name; // Tooltip on hover

                const img = document.createElement('img');
                // Use placeholder image if no logo URL is provided or if it fails to load
                img.src = channel.logo || 'https://placehold.co/60x60/333/fff?text=No+Logo';
                img.alt = channel.name;
                img.classList.add('w-16', 'h-16', 'object-contain', 'rounded-full', 'mb-2', 'flex-shrink-0');
                img.onerror = function() {
                    this.onerror = null; // Prevent infinite loop if fallback also fails
                    this.src = 'https://placehold.co/60x60/333/fff?text=No+Logo'; // Fallback to a generic placeholder
                };

                const name = document.createElement('p');
                name.textContent = channel.name;
                // Truncate long channel names with ellipsis
                name.classList.add('text-sm', 'font-medium', 'text-gray-100', 'truncate', 'w-full');

                channelCard.appendChild(img);
                channelCard.appendChild(name);

                channelCard.addEventListener('click', () => {
                    playSelectedChannel(channel.url, channel.name); // Pass channel name for message
                });

                gridContainer.appendChild(channelCard);
            });
            channelList.appendChild(gridContainer);
        }

        /**
         * Plays the selected channel URL.
         * @param {string} selectedUrl - The URL of the channel to play.
         * @param {string} channelName - The name of the channel to display in the message.
         */
        function playSelectedChannel(selectedUrl, channelName = 'Selected Channel') {
            // Destroy previous HLS instance if it exists to prevent conflicts
            if (currentHls) {
                currentHls.destroy();
                currentHls = null; // Clear the reference
            }

            // Check if HLS.js is supported and if the URL is likely an HLS stream (.m3u8 extension)
            if (Hls.isSupported() && selectedUrl.endsWith('.m3u8')) {
                currentHls = new Hls();
                currentHls.loadSource(selectedUrl);
                currentHls.attachMedia(videoPlayer);
                currentHls.on(Hls.Events.MANIFEST_PARSED, function() {
                    videoPlayer.play();
                    showMessage(`Playing: ${channelName}`);
                });
                currentHls.on(Hls.Events.ERROR, function(event, data) {
                    console.error('HLS.js error:', data);
                    if (data.fatal) {
                        switch(data.type) {
                            case Hls.ErrorTypes.NETWORK_ERROR:
                                showMessage(`Fatal network error for ${channelName}, trying to recover...`, true);
                                currentHls.recoverMediaError();
                                break;
                            case Hls.ErrorTypes.MEDIA_ERROR:
                                showMessage(`Fatal media error for ${channelName}, trying to recover...`, true);
                                currentHls.recoverMediaError();
                                break;
                            default:
                                showMessage(`Fatal HLS error for ${channelName}: ${data.details}`, true);
                                currentHls.destroy();
                                break;
                        }
                    } else {
                        // Non-fatal errors can be just warnings
                        showMessage(`HLS warning for ${channelName}: ${data.details}`, false);
                    }
                });
            } else {
                // For non-HLS streams or if HLS.js is not supported, use native video playback
                videoPlayer.src = selectedUrl;
                videoPlayer.play()
                    .then(() => {
                        showMessage(`Playing: ${channelName}`);
                    })
                    .catch(error => {
                        console.error('Error playing video:', error);
                        showMessage(`Failed to play ${channelName}: ${error.message}. Try a different format or URL.`, true);
                    });
            }
        }

        // Add a listener for native video element errors
        videoPlayer.addEventListener('error', (event) => {
            console.error('Video element error:', event.target.error);
            let errorMessage = 'An unknown video error occurred.';
            switch (event.target.error.code) {
                case event.target.error.MEDIA_ERR_ABORTED:
                    errorMessage = 'Video playback aborted.';
                    break;
                case event.target.error.MEDIA_ERR_NETWORK:
                    errorMessage = 'A network error caused the video download to fail.';
                    break;
                case event.target.error.MEDIA_ERR_DECODE:
                    errorMessage = 'The video playback was aborted due to a corruption problem or because the media used features your browser did not support.';
                    break;
                case event.target.error.MEDIA_ERR_SRC_NOT_SUPPORTED:
                    errorMessage = 'The video could not be loaded, either because the server or network failed or because the format is not supported.';
                    break;
            }
            showMessage(`Video Error: ${errorMessage}`, true);
        });

        // Initial load of all playlists and render UI when the window loads
        window.onload = function() {
            fetchAndParseAllPlaylists();
        };

    </script>
</body>
</html>

