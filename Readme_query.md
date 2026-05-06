### Introduction to MongoDB Queries  

This document provides a series of MongoDB queries and Python functions designed to analyze and filter the collection of Transparent Stays review data. The queries mainly focusses on the collection fields of `AuthorLocation`, `Date`, `Ratings`, and more, enabling the extraction of valuable insights. The code examples and their outputs are intended for execution within the associated Jupyter Notebook, providing a dynamic and interactive environment for data exploration.

Below is a summary of the queries and their purpose:  

#### 1. **Exploring the Elements of the Collection**  
   - **Query**: `collection.find_one()`  
   - **Description**: Retrieves and displays a single document to understand the structure and available fields in the collection.  

#### 2. **Counting Total Reviews**  
   - **Query**: `collection.count_documents({})`  
   - **Description**: Calculates the total number of reviews in the collection.  

#### 3. **Extracting All Available Locations**  
   - **Query**: `collection.distinct("AuthorLocation")`  
   - **Description**: Identifies all unique author locations mentioned in the reviews.  

#### 4. **Extracting Available Dates**  
   - **Query**: `collection.distinct("Date")`
   - **Description**: Extracts and identifies the range of review dates, including the earliest and latest entries.  
        
        dates = collection.distinct("Date")
        print("First review's date:", min(dates))
        print("Last review's date:", max(dates))  

#### 5. **Extracting Ratings Elements from Documents**  
   - **Query**: `collection.distinct("Ratings")`  
   - **Description**: Displays the `Ratings` field from the first two documents to understand its structure and values. 
        
        rating_1 = collection.distinct("Ratings")[0]
        rating_2 = collection.distinct("Ratings")[1]
        print("First document ratings:", rating_1)
        print("Second document ratings:", rating_1) 

---

### Filtering the Reviews  

#### A. **Author Location Filter**  
   - **Purpose**: Normalizes author locations to U.S. states and filters states with over 100 reviews. The first three U.S. locations with the most ammount of reviews were kept.  
   - **Description**: A helper function, `location_normalizer`, processes location strings to match U.S. state names. Reviews are grouped and counted by state.  
        
        # Function to normalize the locations
        def location_normalizer(location):
            # Split the location into words
            location = location.split(",")
            for word in location:
            word = word.strip() # remove white spaces
            try:
                state = us.states.lookup(word)
                if state:
                return state.name
            except ValueError:
                pass # move to the next word
            return "Not in the USA"

        # Locations filter function
        cursor = collection.find({}, {"_id": 0, "AuthorLocation": 1})
        converted_loc = []

        for doc in cursor:
            original_loc = doc.get("AuthorLocation")
            if original_loc:
                norm_loc = location_normalizer(original_loc)
                converted_loc.append(norm_loc)

        # Group and count the reviews
        reviews_counts = Counter(converted_loc)

        # Print the results
        print(f"US location with over 100 reviews:")
        for location, count in sorted(reviews_counts.items(), key=lambda item: item[1], reverse=True):
            if count >= 100:
                print(f"State: {location}, Number of reviews: {count}")


#### B. **Date Range Filter**  
   - **Purpose**: Filters reviews within a specific date range, such as the last five years (2008–2012).  
   - **Description**: The `date_converter` function handles different date formats, and reviews are grouped and counted by year.  

        # Function to normalize Dates formats
        def date_converter(date_str):
            try:
                return datetime.strptime(date_str, "%b %d, %Y")
            except ValueError:
                try:
                    # if datetime format fails try the strptime one (it keeps the whole month)
                    return datetime.strptime(date_str, "%B %d, %Y")
                except ValueError:
                    print(f"Not valid format: {date_str}")
                    return None

        # Dates filter function
        cursor = collection.find({}, {"_id": 0, "Date": 1})
        converted_dates = []

        for doc in cursor:
            original_date = doc.get("Date")
            if original_date:
                converted_date = date_converter(original_date)
                converted_dates.append(converted_date.year)

        # Group and count the reviews
        year_counts = Counter(converted_dates)

        # Print the results
        print(f"Reviews per year:")
        for year, count in sorted(year_counts.items()):
            print(f"Year: {year}, Number of reviews: {count}")

#### C. **Overall Rating Filters**  
   - **Purpose**: Focuses on the `Overall` rating field, verifying its presence in reviews and calculating its distribution (1–5 stars).  
   - **Description**: A function calculates the distribution of overall ratings, helping visualize user feedback trends.  

        # Ratings filter function - to verify that every reviw has the overall rating
        cursor = collection.find({}, {"_id": 0, "Date": 1, "Ratings": 1})
        converted_dates = []

        for doc in cursor:
            original_date = doc.get("Date")
            ratings = doc.get("Ratings", {})

            if original_date and "Overall" in ratings:
                converted_date = date_converter(original_date)
                converted_dates.append(converted_date.year)

        # Group and count the reviews
        year_counts = Counter(converted_dates)

        # Print the results
        print(f"Reviews per year with 'overall' ratings:")
        for year, count in sorted(year_counts.items()):
            print(f"Year: {year}, Number of reviews: {count}")
        
        
        # Function to calculate star distribution
        def get_star_distribution(collection):
            # Query to fetch only the overall rating field
            cursor = collection.find({}, {"_id": 0, "Ratings.Overall": 1})
            ratings = []

            # Collect ratings
            for doc in cursor:
                ratings_field = doc.get("Ratings", {})
                overall_rating = ratings_field.get("Overall")
                if overall_rating is not None:
                    ratings.append(overall_rating)
            
            # Count occurrences of each rating
            star_distribution = Counter(ratings)
            return dict(star_distribution)

        # Print the results
        star_distribution = get_star_distribution(collection_new)
        print("Star Distribution:")
        for star, count in sorted(star_distribution.items()):
            print(f"{star} star(s): {count}")

---

### Conclusion  
The provided queries and functions demonstrate how to extract, filter, and analyze review data from a MongoDB collection effectively. This framework can be extended to include additional filters or insights as required, offering a powerful toolkit for data-driven decision-making.







