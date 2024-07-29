//@version=5
indicator("Chart Pattern Recognition", overlay=true)

// Input settings
patternLookback = input.int(20, title="Pattern Lookback Period")

// Function to find the highest and lowest points in a lookback period
findExtremes(src, length) =>
    [highest(src, length), lowest(src, length)]

// Double Top and Double Bottom identification
doubleTop = na
doubleBottom = na

[highestHigh, _] = findExtremes(high, patternLookback)
[_, lowestLow] = findExtremes(low, patternLookback)

if (ta.valuewhen(high == highestHigh, high, 0) == ta.valuewhen(high == highestHigh, high, 1))
    doubleTop := highestHigh

if (ta.valuewhen(low == lowestLow, low, 0) == ta.valuewhen(low == lowestLow, low, 1))
    doubleBottom := lowestLow

// Head and Shoulders identification
headAndShoulders = na
inverseHeadAndShoulders = na

// Function to check if a given point is a head or shoulder
isHeadOrShoulder(src, peak, trough, len) =>
    peakCond = src[len] > peak and src[len] > src[len + 1] and src[len] > src[len - 1]
    troughCond = src[len] < trough and src[len] < src[len + 1] and src[len] < src[len - 1]
    peakCond or troughCond

// Check for head and shoulders pattern
if (isHeadOrShoulder(high, highestHigh, lowestLow, patternLookback))
    headAndShoulders := highestHigh
if (isHeadOrShoulder(low, highestHigh, lowestLow, patternLookback))
    inverseHeadAndShoulders := lowestLow

// Plot patterns on the chart
plotshape(series=doubleTop, location=location.abovebar, color=color.red, style=shape.labeldown, text="Double Top", title="Double Top")
plotshape(series=doubleBottom, location=location.belowbar, color=color.green, style=shape.labelup, text="Double Bottom", title="Double Bottom")
plotshape(series=headAndShoulders, location=location.abovebar, color=color.orange, style=shape.labeldown, text="H&S", title="Head and Shoulders")
plotshape(series=inverseHeadAndShoulders, location=location.belowbar, color=color.blue, style=shape.labelup, text="Inverse H&S", title="Inverse Head and Shoulders")

<!---
Isurudildhan/Isurudildhan is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
