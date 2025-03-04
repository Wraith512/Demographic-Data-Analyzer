# Demographic-Data-Analyzer
import pandas as pd

def demographic_data_analyzer():
    # Read dataset
    df = pd.read_csv("adult.data.csv")
    
    # Race count
    race_count = df["race"].value_counts()
    
    # Average age of men
    average_age_men = round(df[df["sex"] == "Male"]["age"].mean(), 1)
    
    # Percentage of people with a Bachelor's degree
    percentage_bachelors = round((df["education"] == "Bachelors").mean() * 100, 1)
    
    # Advanced education (Bachelors, Masters, Doctorate)
    advanced_education = df["education"].isin(["Bachelors", "Masters", "Doctorate"])
    
    # Percentage of people with advanced education earning >50K
    higher_edu_rich = round((df[advanced_education]["salary"] == ">50K").mean() * 100, 1)
    
    # Percentage of people without advanced education earning >50K
    lower_edu_rich = round((df[~advanced_education]["salary"] == ">50K").mean() * 100, 1)
    
    # Minimum number of work hours per week
    min_hours_per_week = df["hours-per-week"].min()
    
    # Percentage of people working the minimum hours earning >50K
    min_hours_workers = df["hours-per-week"] == min_hours_per_week
    min_hours_rich = round((df[min_hours_workers]["salary"] == ">50K").mean() * 100, 1)
    
    # Country with the highest percentage of >50K earners
    rich_by_country = df[df["salary"] == ">50K"]["native-country"].value_counts() / df["native-country"].value_counts()
    highest_earning_country = rich_by_country.idxmax()
    highest_earning_country_percentage = round(rich_by_country.max() * 100, 1)
    
    # Most popular occupation in India among >50K earners
    top_occupation_india = df[(df["native-country"] == "India") & (df["salary"] == ">50K")]["occupation"].mode()[0]
    
    return {
        "race_count": race_count,
        "average_age_men": average_age_men,
        "percentage_bachelors": percentage_bachelors,
        "higher_edu_rich": higher_edu_rich,
        "lower_edu_rich": lower_edu_rich,
        "min_hours_per_week": min_hours_per_week,
        "min_hours_rich": min_hours_rich,
        "highest_earning_country": highest_earning_country,
        "highest_earning_country_percentage": highest_earning_country_percentage,
        "top_occupation_india": top_occupation_india
    }

if __name__ == "__main__":
    results = demographic_data_analyzer()
    for key, value in results.items():
        print(f"{key}: {value}")
