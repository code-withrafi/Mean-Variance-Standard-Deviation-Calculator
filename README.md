import pandas as pd

def load_data():
    df = pd.read_csv('adult.csv')
    return df

def race_count(df):
    return df['race'].value_counts()

def average_age_men(df):
    men = df[df['sex'] == 'Male']
    return round(men['age'].mean(), 1)

def percentage_bachelors(df):
    bachelors = df[df['education'] == 'Bachelors']
    return round((len(bachelors) / len(df)) * 100, 1)

def percentage_advanced_education_high_salary(df):
    advanced_education = df[df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])]
    high_salary = advanced_education[advanced_education['salary'] == '>50K']
    return round((len(high_salary) / len(advanced_education)) * 100, 1)

def percentage_no_advanced_education_high_salary(df):
    no_advanced_education = df[~df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])]
    high_salary = no_advanced_education[no_advanced_education['salary'] == '>50K']
    return round((len(high_salary) / len(no_advanced_education)) * 100, 1)

def min_work_hours(df):
    return df['hours-per-week'].min()

def percentage_min_work_hours_high_salary(df):
    min_hours = df[df['hours-per-week'] == df['hours-per-week'].min()]
    high_salary = min_hours[min_hours['salary'] == '>50K']
    return round((len(high_salary) / len(min_hours)) * 100, 1)

def highest_earning_country(df):
    country_group = df.groupby('native-country').apply(lambda x: (x[x['salary'] == '>50K'].shape[0] / x.shape[0]) * 100)
    highest_country = country_group.idxmax()
    highest_percentage = country_group.max()
    return highest_country, round(highest_percentage, 1)

def popular_occupation_in_india(df):
    india_high_salary = df[(df['native-country'] == 'India') & (df['salary'] == '>50K')]
    return india_high_salary['occupation'].mode()[0]

if __name__ == "__main__":
    df = load_data()
    print(race_count(df))
    print(average_age_men(df))
    print(percentage_bachelors(df))
    print(percentage_advanced_education_high_salary(df))
    print(percentage_no_advanced_education_high_salary(df))
    print(min_work_hours(df))
    print(percentage_min_work_hours_high_salary(df))
    print(highest_earning_country(df))
    print(popular_occupation_in_india(df))
