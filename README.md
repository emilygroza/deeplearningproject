# Music Recommendation System

Problem Statement: Music streaming platforms do not give true recommendations of songs personalized to a user’s specific taste

Motivation: I often find myself getting bored of my music, so I often use Spotify’s recommendations to guide my new discoveries. However, Spotify is known to increase a certain artist or songs algorithmic exposure for a commission (https://artists.spotify.com/discovery-mode), not necessarily recommending content purely based on what the user would like. This is why people may find that the same songs are pushed to them when they’re trying to discover new music. I would like to make a music recommendation system truer to what users would actually like. Good personalization algorithms increase user satisfaction and retention

Ideally, for this project I would have liked to use actual user data in addition to song data in order to make recommendations based on what other users like as well. However, my dataset is limited to only the songs data, so I am limited to making this system solely based on the song's qualities.

I am using a dataset from Kaggle that contains data on nearly 114,000 songs from the Spotify API (see https://www.kaggle.com/datasets/priyamchoksi/spotify-dataset-114k-songs). The variables included in this dataset are track_id, artists, album_name, track_name, popularity, duration_ms, explicit, danceability, energy, key, loudness, mode, speechiness, acousticness, instrumentalness, liveness, valence, tempo, time_signature, and track_genre.

### Dataset
Pre-processing shape: (114,000, 21)

Variables: track_id, artists, album_name, track_name, popularity, duration_ms, explicit, danceability, energy, key, loudness, mode, speechiness, acousticness, instrumentalness, liveness, valence, tempo, time_signature, track_genre

Data processing included converting the values for the explicit variables to 0 or 1, removing duplicate track-names, filling missing numeric values with the median value, and choosing the features to create this recommendation system from ("popularity", "duration_ms", "explicit", "danceability", "energy", "key", "loudness", "mode", "speechiness", "acousticness", "instrumentalness", "liveness", "valence", "tempo", "time_signature").

Post-processing shape: (73,609, 15)

### Autoencoder

I tested a variety of embedding dimensions, going from 32 to 64 to finishing with 128. With each change in the embedding dimension, the reconstruction MSE got smaller and smaller, which means the autoencoder trained is really good at reconstructing the input from the learned embeddings. The autoencoder is a shallow, fully connected neural network that takes in 15 standardized numeric features per track, such as popularity, danceability, energy, and tempo. The encoder sends this input through a dense layer with 32 neurons and a dropout of 5% to prevent overfitting, before sending it into a 128-dimensional embedding space via a linear activation layer. The decoder expands the embedding through a 32-neuron dense layer with ReLU activation, then reconstructing the original 15 features in the output layer with a linear activation. The model is trained using the Adam optimizer with a very low learning rate, minimizing mean squared error (MSE) between the input and reconstruction. 

When training the autoencoder, I included early stopping to prevent overfitting. For this, I made the patience to be 5 epochs, so if the model does not improve within 5 epochs, it stops. With this run, it stopped at 38 epochs.

### Embeddings

The autoencoder was trained to learn 128-dimensional embeddings for each track, resulting in an embedding matrix of shape (73,609, 128). To better understand the structure of these embeddings, t-SNE was applied to the first 10,000 tracks and colored by genre. T-SNE was used to reduce 128-dimensional track embeddings to 2 dimensions so we could plot them and see if tracks of similar genres cluster together. If clusters appear by genre, it indicates the embeddings capture meaningful relationships. The resulting visualization showed that tracks of similar genres tended to cluster together, indicating that the embeddings capture meaningful patterns in the numeric features. There wasn't always a clear distinction between the genres as the goal wasn't to define the genres, but show tracks with similar features, which only sometimes aligns with genre shifts.

### Recommendation System

For the recommendations, I built a Nearest Neighbors index using cosine similarity. The recommendation function retrieves top-k similar tracks for a given track ID. Example recommendations for sample tracks show a mix of same and related genres with low cosine distances, reflecting embedding similarity, and showing songs that are similar regardless of genre.

### Evaluation

Evaluation of the model showed a very low reconstruction error, with a mean MSE of 0.0009 and a maximum MSE of 0.1433. The genre hit rate, measured as the proportion of recommended tracks matching the seed track’s genre, was 12.9% for the top 10 recommendations. While increasing the embedding size improved reconstruction accuracy, it slightly reduced the genre hit rate. This suggests that while the embeddings effectively capture detailed numeric similarities, genre alignment is limited by the fact that only numeric features were used, so features were considered regardless of genre.

### Conclusion

In conclusion, the autoencoder compresses the 15 numeric features into meaningful embeddings. The recommendation system reflects both numeric similarity, demonstrating that the embeddings capture important patterns in the data. However, the genre hit rate suggests that including categorical or audio-based features could further improve recommendations by better capturing stylistic similarities between tracks. Additionally, user data and listening histories could help provide a more nuanced and personalized recommendation system in future developments.