# Music Recommendation System

Problem Statement: Music streaming platforms do not give true recommendations of songs personalized to a user’s specific taste

Motivation: I often find myself getting bored of my music, so I often use Spotify’s recommendations to guide my new discoveries. However, Spotify is known to increase a certain artist or songs algorithmic exposure for a commission (https://artists.spotify.com/discovery-mode), not necessarily recommending content purely based on what the user would like. This is why people may find that the same songs are pushed to them when they’re trying to discover new music. I would like to make a music recommendation system truer to what users would actually like. Good personalization algorithms increase user satisfaction and retention

Ideally, for this project I would have liked to use actual user data in addition to song data in order to make recommendations based on what other users like as well. However, my dataset is limited to only the songs data, so I am limited to making this system solely based on the song's qualities.

I am using a dataset from Kaggle that contains data on nearly 114,000 songs from the Spotify API (see https://www.kaggle.com/datasets/priyamchoksi/spotify-dataset-114k-songs). The variables included in this dataset are track_id, artists, album_name, track_name, popularity, duration_ms, explicit, danceability, energy, key, loudness, mode, speechiness, acousticness, instrumentalness, liveness, valence, tempo, time_signature, and track_genre.