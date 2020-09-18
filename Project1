'''
    CITS1401 - Project 1
    Student Name:       Jamie McCullough
    Student Number      22238418

'''
# ----------------------------MATH---------------------------------

# Returns average for a given list.
def avgFor(values):
    sum = 0.0
    
    for i in range(len(values)):
        element = values[i]
        sum = sum + element
    
    if sum <= 0.0 or len(values) == 0:
        return 0.0
    else:
        return sum / len(values)

# Returns standard deviation for a given list.
def stdFor(values):
    avg = avgFor(values)
    sqredMeans = []
    avgMeans = []

    for i in range(len(values)):
        lessAvg = values[i] - avg
        sqrAvg = lessAvg * lessAvg
        sqredMeans.append(sqrAvg)

    avgMeans = avgFor(sqredMeans)

    return avgMeans ** (1.0/2.0)

# -----------------------INPUT CHECKS------------------------------

def negativeCheck(monthRainfall):
    removedNegatives = monthRainfall
    for i in range(len(monthRainfall)):
        if monthRainfall[i] < 0.0:
            removedNegatives[i] = 0.0
    return removedNegatives

# Finds first non-zero value in a given list, if none, returns zero.
def firstNonZero(values):
    for i in range(len(values)):
        if values[i] != 0.0:
            return values[i]
    return 0.0

# --------------------------PARSING--------------------------------

# Returns all values entered for a given year.
def getYearData(csvfile, year):
    yearData = []
    rawData = []

    # Using 'with' forgoes the need of file closure (implicitly closes file)
    with open(csvfile, 'r') as file:
        # Reads data and removes '\n' from file_contents.
        file_contents = file.readlines()
        rows = [x.replace('', '0') for x in file_contents]
        rows = [x.strip() for x in file_contents]

        # Seperates each row into a list by comma and adds them to a list.
        for i in range(1, len(rows)):
            if rows[i] != "":
                commaSeperated = rows[i].split(',')
                rawData.append(commaSeperated)

        # Adds all data relating to given year to a new list.
        for x in range(len(rawData)):
            rowYear = int(rawData[x][1])
            if rowYear == year:
                yearData.append(rawData[x])

    return yearData

# Returns rainfall for a given month
def getMonthRainfall(yearData, month):
    monthlyRainfall = []

    for i in range(len(yearData)):
        if int(yearData[i][2]) == month:
            if yearData[i][4] == '':
                monthlyRainfall.append('0.0')
            else:
                monthlyRainfall.append(yearData[i][4])

    # Converts monthlyRainfall into float list, and then turns any negatives to zero.
    rainfall = negativeCheck([float(x) for x in monthlyRainfall])

    return rainfall

# --------------------------STATISTICS--------------------------------

# Finds largest rainfall in a month of a given years' data
def getMonthMinimum(yearData, month):
    # Returns 0.0 if a year's data is empty (i.e. year not recorded)
    if yearData == []:
        return 0.0

    monthData = getMonthRainfall(yearData, month)
    minimum = firstNonZero(monthData)
    dayRainfall = 0.0

    for i in range(len(monthData)):
        dayRainfall = monthData[i]
        if dayRainfall < minimum and dayRainfall > 0.0:
            minimum = dayRainfall

    return minimum

# Finds largest rainfall in a month of a given years' data
def getMonthMaximum(yearData, month):
    # Returns 0.0 if a year's data is empty (i.e. year not recorded)
    if yearData == []:
        return 0.0

    monthData = getMonthRainfall(yearData, month)
    dayRainfall = 0.0
    maximum = 0.0

    for i in range(len(monthData)):
        dayRainfall = monthData[i]
        if dayRainfall > maximum and dayRainfall > 0.0:
            maximum = dayRainfall

    return maximum

# Uses the avgFor function to get the average for a given month.
def getMonthAverage(yearData, month):
    # Returns 0.0 if a year's data is empty (i.e. year not recorded)
    if yearData == []:
        return 0.0

    monthData = getMonthRainfall(yearData, month)
    return avgFor(monthData)

# Uses stdFor function to get the standard deviation for a given month.
def getMonthStdDev(yearData, month):
    # Returns 0.0 if a year's data is empty (i.e. year not recorded)
    if yearData == []:
        return 0.0

    monthData = getMonthRainfall(yearData, month)
    return stdFor(monthData)

# --------------------------CORRELATION-------------------------------

# Sums the products of the differences between a month and the average.
def sumAvgProducts(y1, y1avg, y2, y2avg):
    # Prevents parsing of missing years.
    if y1 == [] or y2 == []:
        return 0.0

    sum = 0.0

    for i in range(12):
        diff1 = y1[i] - y1avg
        diff2 = y2[i] - y2avg
        sum = sum + diff1 * diff2

    return sum

# Determines the correlation given two years' data.
def correlationFor(y1, y2):
    y1avg, y2avg = avgFor(y1), avgFor(y2)
    y1std, y2std = stdFor(y1), stdFor(y2)

    # Numerator in the equation
    productsOfDiff = sumAvgProducts(y1, y1avg, y2, y2avg)
    # Denominator in the equation
    productsOfStd = (y1std * y2std)

    # Correlation is an improper metric where either of these are zero.
    if productsOfStd == 0.0 or productsOfDiff == 0.0:
        return 0.0
    else:
        corr = (productsOfDiff / productsOfStd) / 12
        return corr

# --------------------------ANALYSIS-------------------------------

# Returns the min, max, avg and std lists
def statsAnalysis(csvfile, year):
    min = []
    max = []
    avg = []
    std = []

    yearData = getYearData(csvfile, year)
    if yearData == []:
        return min, max, avg, std
    else:
        for i in range(1, 13):
            min.append(round(getMonthMinimum(yearData, i), 4))
            max.append(round(getMonthMaximum(yearData, i), 4))
            avg.append(round(getMonthAverage(yearData, i), 4))
            std.append(round(getMonthStdDev(yearData, i), 4))

    return min, max, avg, std

# Returns the min, max, avg, std correlation values
def corrAnalysis(csvfile, year):
    y1min, y1max, y1avg, y1std = statsAnalysis(csvfile, year[0])
    y2min, y2max, y2avg, y2std = statsAnalysis(csvfile, year[1])

    min = round(correlationFor(y1min, y2min), 4)
    max = round(correlationFor(y1max, y2max), 4)
    avg = round(correlationFor(y1avg, y2avg), 4)
    std = round(correlationFor(y1std, y2std), 4)

    return min, max, avg, std

# ----------------------------MAIN---------------------------------

# Handles the statistical requirements of the given arguments
def main(csvfile, year, type):
    if type == "stats":
        return statsAnalysis(csvfile, year)
    elif type == "corr":
        return corrAnalysis(csvfile, year)
