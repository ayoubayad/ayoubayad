//@version=5
indicator("Support and Resistance with Mirrored Numbers", overlay=true)

// Define numbers and their mirrors
numbers = [0, 1, 2, 6, 5]  // الأرقام
mirrors = [9, 8, 7, 3, 4]  // المرايات

// Function to check if a number contains any of the numbers or mirrors
is_mirror_match(value) =>
    str_value = str.tostring(value)
    match_found = false
    for i = 0 to 4
        num = numbers[i]
        mirror = mirrors[i]
        if str.contains(str_value, str.tostring(num)) or str.contains(str_value, str.tostring(mirror))
            match_found := true
    match_found

// Function to find support and resistance
find_support_resistance(length) =>
    support = na
    resistance = na
    // Find local support and resistance levels
    for i = 1 to length
        if low[i] == ta.lowest(low, length)
            support := low[i]
        if high[i] == ta.highest(high, length)
            resistance := high[i]
    [support, resistance]

// Find support and resistance for the last 20 bars
[support, resistance] = find_support_resistance(20)

// Check if support or resistance contains any of the numbers or mirrors
support_match = is_mirror_match(support)
resistance_match = is_mirror_match(resistance)

// Plot support and resistance levels with indicators
plot(support, color=color.green, linewidth=2, title="Support")
plot(resistance, color=color.red, linewidth=2, title="Resistance")

// Add marks if the support or resistance matches the mirror condition
plotshape(series=support_match, location=location.belowbar, color=color.green, style=shape.labelup, title="Support Match", text="Support Match")
plotshape(series=resistance_match, location=location.abovebar, color=color.red, style=shape.labeldown, title="Resistance Match", text="Resistance Match")
