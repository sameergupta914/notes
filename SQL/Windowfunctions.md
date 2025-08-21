# Window functions

- Window functions in SQL are a type of analytical function that perform calculations across a set of rows that are related to the current row, called a "window". A window function calculates a value for each row in the result set based on a subset of the rows that are defined by a window specification. 
- The window specification is defined using the OVER() clause in SQL, which specifies the partitioning and ordering of the rows that the window function will operate on. 
- The partitioning divides the rows into groups based on a specific column or expression, while the ordering defines the order in which the rows are processed within each group.

- Aggregate Function with OVER()
- RANK/DENSE_RANK/ROW_NUMBER
- FIRST_VALUE/LAST VALUE/NTH_VALUE

- Frames:
    - A frame in a window function is a subset of rows within the partition that determines the scope of the window function calculation. The frame is defined using a combination of two clauses in the window function: ROWS and BETWEEN. The ROWS clause specifies how many rows should be included in the frame relative to the current row. For example, ROWS 3 PRECEDING means that the frame includes the current row and the three rows that precede it in the partition. The BETWEEN clause specifies the boundaries of the frame.
        Examples
        - ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW - means that the frame includes all rows from the beginning of the partition up to and including the current row.
       
        - ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING: the frame includes the current row and the row immediately before and after it.
        
        - ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING: the frame includes all rows in the partition.

        - ROWS BETWEEN 3 PRECEDING AND 2 FOLLOWING: the frame includes the current row and the three rows before it and the two rows after it.

- LEAD & LAG
- Ranking
- Cumulative sum- Cumulative sum is another type of calculation that can be performed using window functions. A cumulative sum calculates the sum of a set of values up to a given point in time, and includes all previous values in the calculation.
- Cumulative average- Cumulative average is another type of average that can be calculated using window functions. A cumulative average calculates the average of a set of values up to a given point in time, and includes all previous values in the calculation.

- Running Average- A running average is a statistical method used to calculate the average of data over a moving window. Instead of taking all data points, it focuses on the most recent few values and continuously updates as new data comes in.

    - You first decide the window size (for example, 10 numbers). You calculate the average of those 10 numbers. When a new number comes, you: Drop the oldest number from the window. Add the new number. Recalculate the average for this updated set. This way, the average “moves forward” with time and always reflects latest trends.

    - Example

        Imagine calculating a cricket batsman’s average runs.
        Suppose the window size = 10 matches.
        You calculate the average runs in the last 10 matches.
        After the next match, you:
        Remove the 1st match from the set.
        Add the latest match.
        Recalculate the average again.
        This keeps updating after every new match.

    - Why is it useful?

        Running averages help to smooth out random fluctuations in data.
        They make it easier to spot trends or patterns in noisy or unstable data.

- Percent of totak- Percent of total refers to the percentage or proportion of a specific value in relation to the total value. It is a commonly used metric to represent the relative importance or contribution of a particular value within a larger group or population.
       
- Percent change- Percent change is a way of expressing the difference between two values as a percentage of the original value. It is often used to measure how much a value has increased or decreased over a given period of time, or to compare two different values.

- Percentile and quantiles- A Quantile is a measure of the distribution of a dataset that divides the data into any number of equally sized intervals. For example, a dataset could be divided into deciles (ten equal parts), quartiles (four equal parts), percentiles (100 equal
parts), or any other number of intervals. Each quantile represents a value below which a certain percentage of the data falls. For example, the 25th percentile (also known as the first quartile, or Q1) represents the value below which 25% of the data falls. The 50th percentile (also
known as the median) represents the value below which 50% of the data falls, and so on.

- PERCENTILE_CONT calculates the continuous percentile value, which returns the interpolated value between adjacent data points. In other words, it estimates the percentile value by assuming that the values between data points are distributed uniformly. This function returns a value that may not be present in the original dataset.

- PERCENTILE_DISC, on the other hand, calculates the discrete percentile value, which returns the value of the nearest data point. This function returns a value that is present in the original dataset.

-Segmentation- Segmentation using NTILE is a technique in SQL for dividing a dataset into equal- sized groups based on some criteria or conditions, and then performing calculations or analysis on each group separately using window functions.

- Cumulative distribution- The cumulative distribution function is used to describe the probability distribution of random variables. It can be used to describe the probability for a discrete, continuous or mixed variable. It is obtained by summing up the probability density function and getting the cumulative probability for a random variable