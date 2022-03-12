import time
import pandas as pd
import numpy as np

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    city = input ("choose a city name (chicago, new york city, washington \n) :" ).lower()
    while city not in CITY_DATA.keys():
        print('Enter a valid city')
        city = input ("choose a city name (chicago, new york city, washington \n) :" ).lower()

    # TO DO: get user input for month (all, january, february, ... , june)
    months = ['january', 'february', 'march', 'april', 'may', 'june', 'all']
    while True:
        month = input ("choose month (all, january, february, march, april, may , june) :" ).lower()
        if month in months:
            break;
        print('wrong input')

    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    days = ['monday', 'tuesday', 'wednesday', 'thursday', 'friday', 'saturday', 'sunday', 'all']
    while True:
        day = input ("choose day (all, monday, tuesday, wednesday, thursday, friday, saturday, sunday) :" ).lower()
        if day in days:
            break;
        print('wrong input')

    print('-'*40)
    return city, month, day


def load_data(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    df = pd.read_csv(CITY_DATA[city])
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    df['month'] = df['Start Time'].dt.month
    df['day_of_week'] = df['Start Time'].dt.weekday_name
    df['start hour'] = df['Start Time'].dt.hour
    
    
    if month != 'all':
        months = ['january', 'february', 'march', 'april', 'may', 'june']
        month = months.index(month) + 1
        df = df[df['month'] == month]
        
    if day != 'all':
        days = ['monday', 'tuesday', 'wednesday', 'thursday', 'friday', 'saturday', 'sunday']
        df = df[df['day_of_week'] == day.title()]
    

    return df


def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    print('the common month is : {}'.format(df['month'].mode()[0]))

    # TO DO: display the most common day of week
    print('the common day is : {}'.format(df['day_of_week'].mode()[0]))

    # TO DO: display the most common start hour
    print('the common hour is : {}'.format(df['start hour'].mode()[0]))

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    print('the common start station is : {}'.format(df['Start Station'].mode()[0]))

    # TO DO: display most commonly used end station
    print('the common end station is : {}'.format(df['End Station'].mode()[0]))

    # TO DO: display most frequent combination of start station and end station trip
    df['freq'] = df['Start Station'] + ',' + df['End Station']
    print('the common frequent is : {}'.format(df['freq'].mode()[0]))

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    print('total travel time is : {}',(df['Trip Duration'].sum()).round())

    # TO DO: display mean travel time
    print('mean travel time is : {}',(df['Trip Duration'].mean()).round())

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df,city):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    print(df['User Type'].value_counts().to_frame())

    # TO DO: Display counts of gender
    if city != 'washington':
        print(df['Gender'].value_counts().to_frame())

    # TO DO: Display earliest, most recent, and most common year of birth
    print('the earliest year of birth is : {}',int(df['Birth Year'].min()))
    print('the most recent year of birth is : {}',int(df['Birth Year'].max()))
    print('the most common year of birth is : {}',int(df['Birth Year'].mode()[0]))

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

    
def display_data(df):
    print('the available data :\n')
    
    x = 0
    inp = input('if you like to display a 3 rows of data please enter y for yes if not enter n for no\n').lower()
    if inp not in ['y', 'n']:
        print('wrong answer, please click y for yes or n for no\n')
        inp = input('if you like to display a 3 rows of data please enter y for yes if not enter n for no\n').lower()
    elif inp != 'y' :
        print('Thank you')
    else:
        while x + 3 < df.shape[0]:
            print(df.iloc[x:x+3])
            x += 3
            inp = input('if you like to display a 3 rows of data please enter y for yes if not enter n for no\n').lower()
            if inp != 'y':
                print('Thank you')
                break
    
    
    
def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)

        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df,city)
        display_data(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
	main()
