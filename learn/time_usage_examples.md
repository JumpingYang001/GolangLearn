# Go time Package: Common Functions and Examples

The `time` package provides functionality for measuring and displaying time. It's essential for handling dates, durations, timers, and time-related operations in Go.

## Main Functions and Types
- `time.Now()`: Current local time
- `time.Parse(layout, value string)`: Parses a formatted string into Time
- `time.Unix(sec int64, nsec int64)`: Creates Time from Unix timestamp
- `time.Sleep(d Duration)`: Pauses execution for specified duration
- `time.After(d Duration)`: Returns channel that sends current time after duration
- `time.NewTimer(d Duration)`: Creates a new Timer
- `time.NewTicker(d Duration)`: Creates a new Ticker
- `time.Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location)`: Creates Time from components

## Basic Time Operations Example
```go
package main

import (
    "fmt"
    "time"
)

func main() {
    // Get current time
    now := time.Now()
    fmt.Printf("Current time: %v\n", now)
    
    // Time components
    fmt.Printf("Year: %d\n", now.Year())
    fmt.Printf("Month: %s\n", now.Month())
    fmt.Printf("Day: %d\n", now.Day())
    fmt.Printf("Hour: %d\n", now.Hour())
    fmt.Printf("Minute: %d\n", now.Minute())
    fmt.Printf("Second: %d\n", now.Second())
    fmt.Printf("Nanosecond: %d\n", now.Nanosecond())
    fmt.Printf("Weekday: %s\n", now.Weekday())
    
    // Create specific time
    date := time.Date(2025, time.April, 17, 20, 34, 58, 651387237, time.UTC)
    fmt.Printf("Specific time: %v\n", date)
    
    // Unix timestamp
    fmt.Printf("Unix time: %v\n", date.Unix())
    fmt.Printf("Unix nano: %v\n", date.UnixNano())
}
```

## Time Formatting and Parsing Example
```go
package main

import (
    "fmt"
    "time"
)

func main() {
    // Current time in different formats
    now := time.Now()
    
    // Standard time formats
    fmt.Println("RFC3339:", now.Format(time.RFC3339))
    fmt.Println("RFC822:", now.Format(time.RFC822))
    fmt.Println("Kitchen:", now.Format(time.Kitchen))
    
    // Custom format (using reference time: Mon Jan 2 15:04:05 MST 2006)
    fmt.Println("Custom:", now.Format("2006-01-02 15:04:05"))
    fmt.Println("Date only:", now.Format("January 2, 2006"))
    fmt.Println("Time only:", now.Format("15:04:05"))
    
    // Parsing time strings
    timeStr := "2025-04-17 15:04:05"
    parsedTime, err := time.Parse("2006-01-02 15:04:05", timeStr)
    if err != nil {
        fmt.Println("Error parsing time:", err)
        return
    }
    fmt.Println("Parsed time:", parsedTime)
    
    // Parse with location
    loc, err := time.LoadLocation("America/New_York")
    if err != nil {
        fmt.Println("Error loading location:", err)
        return
    }
    parsedWithLoc, err := time.ParseInLocation("2006-01-02 15:04:05", timeStr, loc)
    if err != nil {
        fmt.Println("Error parsing time with location:", err)
        return
    }
    fmt.Println("Parsed with location:", parsedWithLoc)
}
```

## Duration and Arithmetic Example
```go
package main

import (
    "fmt"
    "time"
)

func main() {
    // Durations
    duration := 2*time.Hour + 30*time.Minute + 45*time.Second
    fmt.Printf("Duration: %v\n", duration)
    
    // Duration components
    fmt.Printf("Hours: %.0f\n", duration.Hours())
    fmt.Printf("Minutes: %.0f\n", duration.Minutes())
    fmt.Printf("Seconds: %.0f\n", duration.Seconds())
    fmt.Printf("Milliseconds: %d\n", duration.Milliseconds())
    fmt.Printf("Nanoseconds: %d\n", duration.Nanoseconds())
    
    // Time arithmetic
    now := time.Now()
    future := now.Add(duration)
    past := now.Add(-duration)
    
    fmt.Printf("Now: %v\n", now)
    fmt.Printf("Future: %v\n", future)
    fmt.Printf("Past: %v\n", past)
    
    // Time difference
    diff := future.Sub(now)
    fmt.Printf("Difference: %v\n", diff)
    
    // Round and truncate
    rounded := duration.Round(time.Minute)
    truncated := duration.Truncate(time.Minute)
    fmt.Printf("Rounded to minute: %v\n", rounded)
    fmt.Printf("Truncated to minute: %v\n", truncated)
}
```

## Timer and Ticker Example
```go
package main

import (
    "fmt"
    "time"
)

func main() {
    // Timer example
    timer := time.NewTimer(2 * time.Second)
    defer timer.Stop()
    
    fmt.Println("Timer started")
    <-timer.C
    fmt.Println("Timer expired")
    
    // Ticker example
    ticker := time.NewTicker(500 * time.Millisecond)
    defer ticker.Stop()
    
    // Count ticks for 2 seconds
    count := 0
    done := make(chan bool)
    
    go func() {
        for {
            select {
            case <-ticker.C:
                count++
                fmt.Println("Tick:", count)
            case <-done:
                return
            }
        }
    }()
    
    // Wait for 2 seconds
    time.Sleep(2 * time.Second)
    done <- true
    fmt.Printf("Ticker stopped after %d ticks\n", count)
}
```

## Time Zones and Locations Example
```go
package main

import (
    "fmt"
    "time"
)

func main() {
    // Get current time in different locations
    now := time.Now()
    
    // Load different locations
    locations := []string{
        "America/New_York",
        "Europe/London",
        "Asia/Tokyo",
        "Australia/Sydney",
    }
    
    for _, locName := range locations {
        loc, err := time.LoadLocation(locName)
        if err != nil {
            fmt.Printf("Error loading location %s: %v\n", locName, err)
            continue
        }
        
        timeInLoc := now.In(loc)
        fmt.Printf("Time in %s: %s\n", locName, timeInLoc.Format(time.RFC3339))
    }
    
    // Working with UTC
    utc := now.UTC()
    fmt.Printf("UTC time: %s\n", utc.Format(time.RFC3339))
    
    // Get zone information
    zone, offset := now.Zone()
    fmt.Printf("Current zone: %s, offset: %d seconds\n", zone, offset)
    
    // Check if time is in DST
    _, isDST := now.Zone()
    fmt.Printf("Is in DST: %v\n", isDST)
}
```

## Comparison and Testing Example
```go
package main

import (
    "fmt"
    "time"
)

func main() {
    time1 := time.Date(2025, time.April, 17, 10, 0, 0, 0, time.UTC)
    time2 := time.Date(2025, time.April, 17, 11, 0, 0, 0, time.UTC)
    
    // Comparing times
    fmt.Printf("time1: %v\n", time1)
    fmt.Printf("time2: %v\n", time2)
    fmt.Printf("Equal: %v\n", time1.Equal(time2))
    fmt.Printf("Before: %v\n", time1.Before(time2))
    fmt.Printf("After: %v\n", time1.After(time2))
    
    // Time until/since
    if time2.After(time1) {
        fmt.Printf("Time until time2: %v\n", time1.Until(time2))
        fmt.Printf("Time since time1: %v\n", time2.Sub(time1))
    }
    
    // Working with monotonic clock
    start := time.Now()
    time.Sleep(100 * time.Millisecond)
    elapsed := time.Since(start)
    fmt.Printf("Operation took: %v\n", elapsed)
}
```

## Important Notes
- Time handling requires careful attention to:
  - Time zones and locations
  - DST (Daylight Saving Time) transitions
  - Leap seconds and years
  - Time format layouts
- The reference time for parsing/formatting is: "Mon Jan 2 15:04:05 MST 2006" (01/02 03:04:05PM '06 -0700)
- Common format constants include:
  - `time.RFC3339`
  - `time.RFC822`
  - `time.RFC1123`
  - `time.Kitchen`
- Duration constants available:
  - `time.Nanosecond`
  - `time.Microsecond`
  - `time.Millisecond`
  - `time.Second`
  - `time.Minute`
  - `time.Hour`
- Always use `time.Time` for storing timestamps, not Unix timestamps
- Use `time.Since()` and `time.Until()` for readable duration calculations
- The zero value for `time.Time` is January 1, year 1, 00:00:00 UTC
- Time comparisons should use `Equal()`, not `==`
- Time operations are safe for concurrent use

See the [official documentation](https://pkg.go.dev/time) for more details.
