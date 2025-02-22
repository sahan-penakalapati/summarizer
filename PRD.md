# YouTube/Podcast Summary Platform

## 1. Create Account & Persistent Login

- Users should be able to create an account and log in.
- Once logged in, users shouldn’t have to log in multiple times on the same browser (session persistence).
- User should be able to log out once logged in. We clear the session persistence when the user logs out.
- [Need to plan how the experience should be for the user when they are not logged in vs logged in (what is visible on the home page, what functionality is available, etc.).]

## 2. Link Submission on Home Page

- There will be a landing page, which is static in nature. We highlight the value proposition of the product.
  - Top Navigation Bar
    - Logo on the left
  - Signup and Login buttons will be present on the landing page
- On Signup, we collect the following details
  - Name
  - Email
  - Password
- On Login, we collect the following details
  - Email
  - Password
- When users log in, the **Home Page** should provide an input field to submit a YouTube link for summary generation.
- Home Page should have the following
  - Top Navigation Bar
    - On the left, we have the logo on the left.
    - On the right, we have the following options
      - Home (active by default)
      - Subscribe
        - Search Bar to search for a channel
        - List of channels
      - Past Summaries
        - Search Bar to search for a summary
        - List of summaries by date
        - Ability to delete a summary
      - Profile
        - My Account. Users can update the following details and save the changes:
          - Name
          - Email
          - Password
        - My Subscriptions
          - List of channels that the user is subscribed to
        - Logout
- Subscribe Page
  - Users will see a list of channels that they can subscribe to
  - Each channel will have the following details
    - Name
    - Description
    - Subscribe button
    - Upvote and Downvote buttons
    - Number of youtube subscribers [not sure about this - what if tomorrow we start for articles or blogs or podcasts]
  - Users can subscribe to a channel by clicking on the subscribe button. On clicking the subscribe button, the user is subscribed to the channel and the button text changes to Unsubscribe. [there should be a cool animation on subscribe]
  - Users can unsubscribe from a channel by clicking on the unsubscribe button. On clicking the unsubscribe button, the user is unsubscribed from the channel and the button text changes to Subscribe.
  - Users can see the list of videos/podcasts for a channel by clicking on the channel name
    - We show the list of videos/podcasts in a grid layout [what all will come on this card?]
    - Users can click on a video/podcast to see the summary card
    - Users can click on the subscribe button to subscribe to a channel
    - Users can click on the unsubscribe button to unsubscribe from a channel

## 3. Submit YouTube/Podcast Link & Generate Summary

- After creating an account, users should be able to submit a YouTube link (which could be of multiple types)
- We validate the youtube link immediately on the client side
- User can click on 'Generate Summary' button to generate the summary
- We show a skeleton loader of Summary Card on button click
- We show the summary card on success
- The system should generate a summary and relevant tags for that particular video
- On the summary card, the user should see:
  - **The name/title** of the video or podcast.
  - **The channel name**.
  - **The creation date** of the video or podcast.
  - **Subscribe** button, which allows the user to subscribe to the channel.
  - **Show Transcript** button, which allows the user to see the transcript of the video. We show a skeleton loader on button click. The button is disabled until the transcript is shown. The reason for this is that the transcript and summaries are shown on the same card and we don't want to show the transcript again if it is already shown. [get transcript can also be separate page it seems]
  - **Show Detailed Summary or Show Brief Summary** button, which allows the user to see the detailed summary of the video. We generate the detailed summary on button click if it is not already generated. We show a sceleton loader on button click. Once the detailed summary is generated, the button text changes to 'Show Brief Summary'. When user clicks on this button, we show the brief summary of the video on the card that was generated previously. [brief summary nis not needed!]
  - **Tags generated for that summary/content.** [tags will be same for a summary/video - essentially link tags to a content piece and not the summary]
  - **Names of the people who are featured in the video/podcast.**
- We also show an option to see the transcript of the video. On clicking 'Show Transcript', we show the transcript of the video on the card.
- We show the option to generate a detailed summary on the summary card. On clicking 'Show Detailed Summary', we show the detailed summary of the video on the card.
- Keep all the buttons in the summary card in the same place and at the top of the card.
- The content should be well formatted and easy to read.
- If the summary for a video has already been generated, display the existing summary to save on API/LLM costs. [same for transcript. if for a content piece we already have content pulled in, we should not pull it again. retreiving the same summary will also result in a much faster experience. also, will have to do a lot of prompt engineering to improve the quality of the summary to get names, tags, main points, examples mentioned properly]

## 5. Interactive Q&A with Transcript

- After reading a summary, users should be able to **chat** with the video or podcast, asking very specific questions.
- An **integrated LLM** should provide answers based on the content of the transcript.
- Chat interface should be present below the summary card. [ this should be like commenting on a post on linkedin where once you put your comment, you get llm's response as a reply]
- User should be able to ask questions about the video/podcast.
- User should be able to ask follow up questions. [ensure context for followup questions is transcript + previous question and answer]
- User should be able to ask questions in natural language.
- User should be able to hide the chat interface.

## 6. Tags & Search Functionality

- Each **summary card** should display **tags extracted** from the video or podcast.
- Clicking on a **tag** should open a **search page** displaying other videos/podcasts with the same tag on the Subscribe page.
- Users can search for videos/podcasts on the Subscribe page using the following:
  - **Title of the video/podcast**
  - **Channel name**
  - **Terms from the summaries**.
  - **Tags associated with the video**.
  - **Names of the people featured in the summary**.
- In **search suggestions**, the system should show **tags already in the database** to facilitate search.

## 7. Past Summaries

- We show the list of summaries using the same card as the summary card in the past summaries page.
- Users can delete a summary by clicking on the delete button. We show a confirmation dialog before deleting the summary. Note that we only delete the summary from the past summaries page of the user and not from the summaries table in the database.
- Users can search for summaries using the following: [i think this feature itself is not needed]
  - **Title of the video/podcast**
  - **Channel name**
  - **Terms from the summaries**.
  - **Tags associated with the video**.
  - **Names of the people featured in the summary**.

## 7. Automated Content Updates & Notifications

- A **cron job** should periodically check for **new videos or podcasts** in all channels on the platform.
- Users should receive a **daily email** listing new content summaries from their subscribed channels.
- If **there is no new content** for a user on a given day, **no email** should be sent.

## 8. Channel Discovery & Ranking

- There should be a **page listing all popular channels** (both YouTube and podcasts) where users can:
  - **Subscribe** to channels to see only summaries from those channels.
  - **Upvote or downvote** channels, establishing a ranking.
  - **Click on a channel** to view any existing summaries for that channel.

## 9. Content Details

- When a user posts a URL, we show the summary that was generated for that content even if the summary was not generated by the same user.
- For every URL or video/podcast, all the details of the content that we show on the summary card should be stored in the database. Following are the details that we store:
  - Title
  - Channel Name
  - Creation Date
  - Transcript
  - Brief Summary
  - Detailed Summary
  - Tags
  - Names of the people featured in the summary [also companies mentioned in the summary?]
- The interactive Q&A with the transcript also should be stored in the database. However, the Q&A with a transcript is attached to the same user who had the Q&A with the transcript. For example, if user A has the Q&A with the transcript of video A, then the Q&A with the transcript of video A should be attached to user A. However, that is not the case for the details of summary card.

## 10. Edge Cases

- Empty States or Error States should be handled gracefully.
- Invalid or unsupported link submissions should be handled gracefully.
- Chunking of the transcript should be done based on the length of the transcript. [why is chunking required?]

## 10. Future Enhancements

- We will add a **feed** of all the summaries from the channels that the user is subscribed to. [on home page right? let's do this in v1]
- We will add a **feed** of all the summaries based on the topics that the user is interested in.[not sure of this]
- User should be able to share the summary card on social media.
- We will add a **search bar** to the top navigation bar to search for videos/podcasts. [this is alreadt there right]
- We will enable other content formats like articles, newsletters, books, podcasts, etc.
- Build more content formats like **Quizzes** and **Flashcards** for better retention.
- We will enable users to **generate summaries** for other content formats.
- We will enable users to **ask questions** about other content formats.
- We will enable users to **search for summaries** for other content formats.
- We will enable users to **subscribe to channels** for other content formats.
- Get a complete picture of the user's content consumption.
- Get a complete picture of the people the user is interested in.
- Have dedicated pages for **Topics** and **People**.
- Get a complete picture of the user's interests and preferences. This will help us in showing the most relevant content to the user.
- As a user, I want to save my favorite summaries so that I can revisit them easily later.
- As a channel owner, I want insights into how users interact with my content’s summaries so that I can improve my videos/podcasts accordingly.
- As a user, I want to filter search results by date or relevance so that I can find recent or most useful content quickly.
- Integrate posthog for analytics.

Comments:

- We should let users generate summaries without logging in too? Or maybe just view summaries of certain channels/podcasts so they get a flavour of what to expect.
- see this product once:

  - https://www.youtubesummaries.com/
  - https://summarize.ing/
  - https://chatgpt4youtube.com/pricing
  - https://www.notta.ai/en/blog/youtube-video-summarizer#youtube-summarizer

  Validation of this tool:

  - https://www.reddit.com/r/ChatGPTPro/comments/1ahabpq/is_it_possible_to_automate_getting_summaries_of/

  General read:

  - https://www.reddit.com/r/ChatGPT/comments/185uxkh/whats_the_best_ai_youtube_video_summarizer_you/
