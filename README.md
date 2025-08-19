# Lightweight YouTube Video Tracking for Google Tag Manager

A lightweight, standalone JavaScript solution for tracking essential YouTube video interactions within Google Tag Manager. This script is designed as a simple and highly reliable alternative to GTM's built-in trigger, focusing only on the core user actions: **start, pause, and complete**.

The script automatically detects all YouTube `<iframe>` players, attaches listeners, and pushes interaction data to the `dataLayer`. It uses a custom event name (`video`) but formats the data to be **fully compatible with GTM's native Built-In Video Variables**, giving you the best of both worlds: a simple script and an easy setup.

## Key Features

-   **Minimalist Event Tracking**: Captures only the most important user actions, keeping your data clean and focused:
    -   `start` (on first play)
    -   `pause`
    -   `complete` (on video finish)
-   **GTM Native Variable Compatibility**: Pushes data using keys like `gtm.videoTitle` and `gtm.videoStatus`, allowing you to use GTM's standard `{{Video Title}}`, `{{Video URL}}`, and `{{Video Status}}` variables without any custom configuration.
-   **API-Free Metadata**: Achieves high reliability by sourcing video metadata directly from iframe DOM attributes, avoiding potential API timing issues:
    -   **Video Title**: Fetched from the iframe's `title` attribute.
    -   **Video URL**: Constructed from the iframe's `src` attribute.
-   **Automatic Player Detection**: Finds and attaches to all YouTube iframes on a page.
-   **No External Dependencies**: Pure JavaScript with no libraries required.

## How to Implement

Follow these steps to set up the tracking in your Google Tag Manager container.

### Step 1: The Listener Tag (The Script)

This tag listens for video interactions.

1.  In GTM, go to **Tags** > **New**.
2.  Name the tag `Custom HTML - YouTube Video Listener (Lightweight)`.
3.  Choose **Custom HTML** as the tag type.
4.  Copy and paste the entire script content from the `.js` file into the HTML field.
5.  Go to the **Triggering** section and select the **Window Loaded** trigger.
6.  Save the tag.

### Step 2: Enable Built-In Video Variables

This step simplifies your entire GTM setup.

1.  In GTM, go to **Variables**.
2.  Under the **Built-In Variables** section, click **Configure**.
3.  Scroll down to the "Video" section and ensure the following variables are enabled:
    -   `Video Provider`
    -   `Video Status`
    -   `Video Title`
    -   `Video URL`

### Step 3: Create the Trigger

This trigger will fire whenever our script pushes a `video` event.

1.  In GTM, go to **Triggers** > **New**.
2.  Name the trigger `Custom Event - video`.
3.  Choose **Custom Event** as the trigger type.
4.  In the **Event name** field, enter `video`.
5.  Leave "All Custom Events" selected.
6.  Save the trigger.

### Step 4: Create the Reporter Tag (e.g., GA4 Event)

This tag sends the captured data to your analytics platform.

1.  In GTM, go to **Tags** > **New**.
2.  Name the tag `GA4 Event - Video Interaction`.
3.  Choose **Google Analytics: GA4 Event** as the tag type.
4.  Select your GA4 Configuration Tag.
5.  For the **Event Name**, you can use a static name like `video_interaction` or make it dynamic using the built-in status variable: `video_{{Video Status}}`.
6.  Under **Event Parameters**, add the following rows:
    | Parameter Name | Value |
    | :--- | :--- |
    | `video_title` | `{{Video Title}}` |
    | `video_url` | `{{Video URL}}` |
    | `video_status`| `{{Video Status}}` |
7.  For **Triggering**, select the `Custom Event - video` trigger you created in Step 3.
8.  Save the tag.

## Data Layer Structure

The script pushes the following object to the `dataLayer` for each interaction.

**Example `start` event:**
```json
{
  "event": "video",
  "gtm.videoProvider": "youtube",
  "gtm.videoStatus": "start",
  "gtm.videoTitle": "The Video Title From The Iframe Attribute",
  "gtm.videoUrl": "https://www.youtube.com/watch?v=VIDEO_ID"
}
