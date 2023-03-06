# IP Clock

This is a final project for the CS50P Introduction to programming with Python.
The project is aimed toward understanding APIs and graphics libraries.

## Demo

Video Demo: <URL>

## Description

The IP Clock gets the user's IPv4 from [ipify]("https://www.ipify.org/") API and uses and sends that
address to [TimeAPI]("https://www.timeapi.io/") to fetch time at their timezone.
The hour, minute, and
second are converted to degrees and drawn using the [turtle]("https://docs.python.org/3/library/turtle.html") module.
Since drawing the shapes is intensive, the clock hands are only redrawn when the time has changed.
The delay time for the main event loop is set to 0.5 seconds, and can be changed to update slower but
must be greater than 0.



Sending GET request to ipify API:

```
def get_ip():
    # Get IP
    ip_response = requests.get('https://api64.ipify.org?format=json').json()
    return ip_response["ip"]
```

Sending GET request with IP to timeAPI:

```
def get_time(ip):
    # Get time
    url = 'https://www.timeapi.io/api/Time/current/ip?ipAddress=' + ip
    response = requests.get(url)
    data = response.json()
```

Returning requested data:

```
return {"hour": data['hour'], "minute": data['minute'], "second": data['seconds'],
        "ip": ip, "timezone": data['timeZone']}
```

Main loop in draw_clock that updates time and redraws clock:

```
    while True:
        while second != current_info["second"]:
            second = current_info["second"]
            second_hand.reset()
            draw_second_hand(second)

            if hour != current_info["hour"]:
                hour = current_info["hour"]
                minute = current_info["minute"]
                hour_hand.reset()
                draw_hour_hand(hour, minute)

            if minute != current_info["minute"]:
                minute = current_info["minute"]
                minute_hand.reset()
                draw_minute_hand(minute)

        current_info = get_time(ip)
        sleep(0.5)
```

Example drawing of the second hand on the clock:

```
def draw_second_hand(second):
    # Calculate minute angle
    angle = 6 * second

    # Draw second hand
    second_hand.hideturtle()
    second_hand.speed(0)
    second_hand.color("red")
    second_hand.left(90)
    second_hand.right(angle)
    second_hand.forward(70)
    second_hand.write(second, False, align='center', font=('Helvetica', 12, 'bold'))
    return angle
```

Simple tests to check angle calculations:

```
def test_hour_angle():
    assert draw_hour_hand(6, 30) == 195


def test_minute_angle():
    assert draw_minute_hand(30) == 180


def test_second_angle():
    assert draw_second_hand(15) == 90
```

## Getting Started

### Dependencies

```
pip install -r requirements.txt
```

### Executing Program

```
python project.py
```

## Authors

Contact email: [Micah Ahn](mailto:micah.kiyoshi@gmail.com)

## Version History

* 0.1
    * Initial Release

## License

See LICENSE.txt

## Acknowledgements

Thank you to the CS50P team!

## TODO

This program has not been tested on Windows OS yet. Feel free to provide input.
