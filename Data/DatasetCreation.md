Creating the dataset
================
DLW
5/20/2020

``` r
dat <-
  r_data_frame(
    n = 300,
    id = id,
    # 2 groups
    athlete = dummy,
    smokes = smokes,
    live_on_campus = dummy,
    # 3+ groups
    race = race, 
    sex = sex_inclusive,
    `grade_class` = level,
    political = political,
    # Scale variables
    height = height,
    iq = iq(mean = 103.5),
    hs_gpa = normal(mean = 3.1, sd = .3, min = 2, max = 4),
    r_series(normal, j = 2, name = "weight", mean = 170, sd = 15, relate = "+5_5"),
    `age` = rpois(lambda = 3), #Need to add 17 to this value
    r_series(normal, j = 4, name = "ACT", mean = 21, sd = 5.4, min = 1, max = 36, relate = "+.3_6"),
    r_series(likert, 5, name = "SWLS_time1", relate = "-.5_.1"),
    r_series(likert, 5, name = "SWLS_time2", relate = "-.5_.1"),
    r_series(normal, j = 3, name = "exam", mean = 75, sd = 10, min = 0, max = 100, relate = "+5_5")
    ) %>%
  mutate_at(vars(starts_with("SWLS")), rescale, to = c(1, 5)) %>% # Comment 1 below
  mutate_at(vars(starts_with("SWLS")), round, digits = 0) %>% 
  mutate_at(vars(starts_with("ACT")), rescale, to = c(1, 36)) %>% 
  mutate_at(vars(starts_with("Exam")), rescale, to = c(0, 100)) %>% 
  mutate_at(vars(starts_with("ACT"), starts_with("weight"), starts_with("Exam")), 
            round, digits = 0) %>%
  rename(ACT_English = ACT_4, # Comment 2 below
         ACT_Mathematics = ACT_3,
         ACT_Reading = ACT_1,
         ACT_Science = ACT_2)
```

# Comment 1

Unfortunately, the relate argument in the r\_series option messes up the
range of the response scale. If you ran the above code for SWLS without
the mutate\_at function below, then this would be the response ranges
for the five items:

1.  Item\_1: 1-5
2.  Item\_2: 0-5
3.  Item\_3: -1-5
4.  Item\_4: -2-5
5.  Item\_5: -3-4

Clearly, the response scales should always be 1-5. Therefore, needed to
rescale using the scales::rescale package into being 1-5.

# Comment 2

ACT scores range from 1-36 and the average composite score is 21 (sd =
5.4). I did the relate function so that the scores went up by subscale
slightly and then renamed and ordered by what students typically get
highest scores on (english) to lowest (science).

``` r
# So I can quickly see the data summary
print(dfSummary(dat, plain.ascii = FALSE, style = "grid", valid.col = FALSE), method = 'render')
```

<!--html_preserve-->

<div class="container st-container">

<h3>

Data Frame Summary

</h3>

<h4>

dat

</h4>

<strong>Dimensions</strong>: 300 x 31 <br/><strong>Duplicates</strong>:
0 <br/>

<table class="table table-striped table-bordered st-table st-table-striped st-table-bordered st-multiline ">

<thead>

<tr>

<th align="center" class="st-protect-top-border">

<strong>No</strong>

</th>

<th align="center" class="st-protect-top-border">

<strong>Variable</strong>

</th>

<th align="center" class="st-protect-top-border">

<strong>Stats / Values</strong>

</th>

<th align="center" class="st-protect-top-border">

<strong>Freqs (% of Valid)</strong>

</th>

<th align="center" class="st-protect-top-border">

<strong>Graph</strong>

</th>

<th align="center" class="st-protect-top-border">

<strong>Missing</strong>

</th>

</tr>

</thead>

<tbody>

<tr>

<td align="center">

1

</td>

<td align="left">

id \[character\]

</td>

<td align="left">

1.  001
2.  002
3.  003
4.  004
5.  005
6.  006
7.  007
8.  008
9.  009
10. 010 \[ 290 others \]
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    290
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    96.7%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJ0AAAESBAMAAAAVmcpcAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAKpJREFUeNrt1UEBg1AMBUEkFAdNkYB/b1yqAN4h4c8KmOtum55We6YPj8fj8XgPvBC3rFc8Ho/H4zXwQtwYr3g8Ho/Ha+CFuGW94vF4PB6vgRfixnjF4/F4PF4DL8SN8YrH4/F4vBd6IW6MVzwej8fjNfBC3BiveDwej8dr4IW4Zb3i8Xg8Hq+BF+LGeMXj8Xg8XgMvxC3r/UJ9/95xZuLxeDxeby/9D93vAo621Uy/ncSwAAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAw8Ku/1wAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMIH2B2sAAAAASUVORK5CYII=">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    2
    </td>
    <td align="left">
    athlete \[numeric\]
    </td>
    <td align="left">
    Min : 0 Mean : 0.5 Max : 1
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    0
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    136
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    45.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    164
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    54.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAF4AAAA4BAMAAACGUVIaAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAEFJREFUSMdjYBj6QIkgUBQEA6h6ZWNCYFT9qPpR9YNVPan5XZBYMEjVE/YvFBAdnhBgNKp+VP2o+kGnntT8PpQBAGj/Uro4JOe+AAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAw8Ku/1wAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMIH2B2sAAAAASUVORK5CYII=">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    3
    </td>
    <td align="left">
    smokes \[logical\]
    </td>
    <td align="left">
    1.  FALSE
11. TRUE
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    252
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    84.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    48
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    16.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAIoAAAA4BAMAAADQq30pAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAEpJREFUSMdjYBgFuIASZQBqirIxJcBo1JRRU0ZNGTVl1BQSTaFO6S1IGRi2piAHlSLZpiDH9Kgpo6aMmjJqyqgptDeFOqX3KMAEAElamNqz+5ELAAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAw8Ku/1wAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMIH2B2sAAAAASUVORK5CYII=">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    4
    </td>
    <td align="left">
    live\_on\_campus \[numeric\]
    </td>
    <td align="left">
    Min : 0 Mean : 0.5 Max : 1
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    0
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    157
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    52.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    143
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    47.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAFsAAAA4BAMAAABgeJleAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAD9JREFUSMdjYBjaQIk4oABVrmxMFBhVPqp8VPkAKycxawsSBwQGo3Ji/ImknIiANBpVPqp8VPnAKycxaw9VAAAx009aYFRqYwAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    5
    </td>
    <td align="left">
    race \[factor\]
    </td>
    <td align="left">
    1.  White
12. Hispanic
13. Black
14. Asian
15. Bi-Racial
16. Native
17. Other
18. Hawaiian
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    177
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    59.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    56
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    18.7%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    46
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    15.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    15
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    5.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    1.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    1.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    0
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    0
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAGUAAADKBAMAAABOGJPHAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAJBJREFUaN7t1MEJgDAQRNGUoB0Y7UD7782LhwQC2V1B2eHPeR8kc5hS9LI7sj3muOzBYDDaJrIhqyPLx6Z/cTWZvjcMBqNiInuwOpLBtA1YTdP1icFgdExkD2Z3fxrLPwa9TfvCYDD5TGQPrPdZjLkADAaDGRgHYd8wGEwq4yApTMVgMJgXxkHYNwwGI2+UcgOFiBDCbChXGwAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    6
    </td>
    <td align="left">
    sex \[factor\]
    </td>
    <td align="left">
    1.  Male
19. Female
20. Intersex
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    99
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    33.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    105
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    35.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    96
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    32.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAEEAAABQBAMAAAC0UwVIAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAExJREFUSMdjYBg8QAkXEBSAqlA2xg6MRlWMqqCbCsLpVBAnoK8KnC5VUiDgW2PjURWjKgaPCsIpebDnOUWCJcyoilEV9FNBOJ0OBgAA3JJKiaXqJ3QAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDDwq7/XAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAwgfYHawAAAABJRU5ErkJggg==">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    7
    </td>
    <td align="left">
    grade\_class \[integer\]
    </td>
    <td align="left">
    Mean (sd) : 2.6 (1.1) min \< med \< max: 1 \< 3 \< 4 IQR (CV) : 2
    (0.4)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    60
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    20.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    85
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    28.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    76
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    25.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    79
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    26.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAADcAAABoBAMAAACnLXVXAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAFhJREFUSMft07sNACAMQ0FGgA0gbAD774YQNEHBHZ/Cr70qkuPcm0SXQs9PzFVFJFoINxSsDqNYRfuUEZGoEW7ok1Gn3X/2iMQV4YY+GbWgUwqRuCDc0O0amL5SRUJXkwYAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDDwq7/XAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAwgfYHawAAAABJRU5ErkJggg==">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    8
    </td>
    <td align="left">
    political \[factor\]
    </td>
    <td align="left">
    1.  Democrat
21. Republican
22. Constitution
23. Libertarian
24. Green
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    171
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    57.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    128
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    42.7%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    0
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    0
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.0%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 5px 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px 0 0;border:0;" align="left">
    (
    </td>
    <td style="padding:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 2px;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAGIAAACABAMAAAACB6G0AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAGBJREFUWMPt1MENgCAQRUFasAS0A+2/Ny/ePLCbGBAy7z4J2YRfyjrt8R5xXNFOgiB+IfL/fIvXUbQeX1+idSuCIOYX+WXIr08PUQmCID4QcWDhCIKYTcSBhSMIYqxYoRs17iUstVNf+wAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    9
    </td>
    <td align="left">
    height \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 69 (4) min \< med \< max: 57 \< 69 \< 79 IQR (CV) : 6
    (0.1)
    </td>
    <td align="left" style="vertical-align:middle">
    22 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAKNJREFUaN7t2csNgCAQRVFakA7EDqT/3owzC0lGkd9GvG/hAuFkIDEScI60ZHmND5Jsn3IsntnByjBd+kHYJgxYK5b9DmqxbH1gYGBgYGBgYGC/xXTTaTFpXisxYaLF5DkD5q/VGoBdDBgYGNjsWDC/lQ5sM/WBgYF9BfPJ6XM/ZvqDgYH9HUsvVboxffkRLCTpxh5GFWNaxiBMm2+m2R5HWnIAtqtiquXN9iAAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDDwq7/XAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAwgfYHawAAAABJRU5ErkJggg==">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    10
    </td>
    <td align="left">
    iq \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 104.2 (9.8) min \< med \< max: 73 \< 104 \< 131 IQR (CV)
    : 15 (0.1)
    </td>
    <td align="left" style="vertical-align:middle">
    45 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAALFJREFUaN7t2d0NhCAQRlFa0A6EDqT/3tYFI4h/6zAa3dzvgYSHORlNiBk0hkjSbMd+0zW/5BhzfogqFtqzSlhYejCwu7B253idx/z28foTLLwuLWxawMDAwMAeiMVPpBbmJwcM7PXY+vgjxOIeDAwMDAwMTB2z6QqjHnNFHRgYGNgLsTTlH2LlzLPEiuI9rGwPDOwyLPvbU4+F/QzLbgNEWKwfMZceX4TF/ayz2hgiyQcFH1fk6KOfPQAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    11
    </td>
    <td align="left">
    hs\_gpa \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 3.1 (0.3) min \< med \< max: 2.2 \< 3.1 \< 4 IQR (CV) :
    0.4 (0.1)
    </td>
    <td align="left" style="vertical-align:middle">
    299 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAIpJREFUaN7t18ENgCAMQNGuoCPgBrL/bibSSyOpWhr08P+Fi7w0kQOIUKTltrW0vG+eY/VsBwObgXln9zXmDQgG9hNMz3wOthkFDAwMDAwMDAwMDOyKdZ+dHqYbulh3QA9rSgUDAwMDAwMDs5eMUcwo8zB7sxrE7IBgX2H6U3MwXcDCWDGlYtGEIh1nA0N6yEJUIAAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    12
    </td>
    <td align="left">
    weight\_1 \[variable, numeric\]
    </td>
    <td align="left">
    Mean (sd) : 171.3 (14.2) min \< med \< max: 134 \< 171 \< 217 IQR
    (CV) : 19 (0.1)
    </td>
    <td align="left" style="vertical-align:middle">
    67 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAJFJREFUaN7t2UEKgCAQQFGvUDeobpD3v1sUMwuFtJwJlP7fBJGPwBYOhUAtTeVWqfLYM2yLVzsYWPeYfvkumCARDAwMDAwMbERsTo8FRixFwHrBKmPPOyyW3xAMDAwMDAwMDAxsGEzOyYsLJlewMx1BXLBsFZg7lk+MJkxv/xPL/7yYsJtVYJ9hun3ZbtoK1NIB1xxJcp+NbD4AAAAldEVYdGRhdGU6Y3JlYXRlADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDDwq7/XAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAwgfYHawAAAABJRU5ErkJggg==">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    13
    </td>
    <td align="left">
    weight\_2 \[variable, numeric\]
    </td>
    <td align="left">
    Mean (sd) : 176.2 (14.8) min \< med \< max: 136 \< 176 \< 217 IQR
    (CV) : 20 (0.1)
    </td>
    <td align="left" style="vertical-align:middle">
    66 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAJRJREFUaN7t2M0JwCAMhmFXaDdo3aDZf7dCSQ5K6/9B6vtdBDEPgqAS50hLtnROTWZZGeblyQUGBgYGtg5mD8kQTBExLPNK1WGS3iEYGBgYGBgYGBjYNNge/rI7sRABmw3Twz6GYDqCgYGB1WFxA6YLs2kwMLB1MbtUhmD+dfVvsLi9XodZtWIfVYWYTUc764sjLbkBDA5LfhwzBSwAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDDwq7/XAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAwgfYHawAAAABJRU5ErkJggg==">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    14
    </td>
    <td align="left">
    age \[integer\]
    </td>
    <td align="left">
    Mean (sd) : 2.9 (1.7) min \< med \< max: 0 \< 3 \< 9 IQR (CV) : 2
    (0.6)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    0
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    19
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    6.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    45
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    15.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    74
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    24.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    71
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    23.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    42
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    14.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    27
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    9.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    6
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    12
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    4.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    7
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    6
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    2.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    8
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    1.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    9
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    0.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAADIAAAD6BAMAAAARqglbAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAJZJREFUWMPt1sENgCAQRFFa2BKEDpb+ezMmGMF1OBjBZJm5vssefgghzFs8Js2KpJyzUhYR3IHYDZF4bbNXl1HcC+5gWG/n7G1lSnEruIMfXr4yQVcrxa/gDob1dpNUHUXxLriDyS/fg6T2YoprwR28qQpL9ae0t1GWEdyB2A0qEUlSyjqCOxD5UiKF0hWZJBuF0pUZ2wFaO/apTi7H9gAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    15
    </td>
    <td align="left">
    ACT\_Reading \[variable, numeric\]
    </td>
    <td align="left">
    Mean (sd) : 20.1 (7) min \< med \< max: 1 \< 21 \< 36 IQR (CV) : 9
    (0.4)
    </td>
    <td align="left" style="vertical-align:middle">
    34 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAIpJREFUaN7t2cEJwCAMhWFXaDeoblD3362XJpcglRiKkP+dBM13UZBoKcST4zNnlYzXzGP9zQ32K6abGIE1McDAwMDAwDbG5O67IjAZgIGBzWHaQ4RgUgIGBgYGBgYGBgaWGTNvvCuYqU2P2X+QBUxnkmJ12BE6sDasBQPbBNMTH4HpzPZYDUkhnjx9Z46dPlNORQAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    16
    </td>
    <td align="left">
    ACT\_Science \[variable, numeric\]
    </td>
    <td align="left">
    Mean (sd) : 18.1 (6) min \< med \< max: 1 \< 18 \< 36 IQR (CV) : 8
    (0.3)
    </td>
    <td align="left" style="vertical-align:middle">
    32 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAIpJREFUaN7t2MEJwCAMQFFXsBtUN6j77+ahjYdKiJgcSvn/JIgP8RRMiXbKakXKdiZW290FBgYGBgYGBhaHHTKvhGCP0cAc2MIIuY41+4pgYGBgYGBgYH/GZLQ6IzBZ2NgY6SKw6SwYGBgYGBgY2MewMn0xOrCqntWx+UPNgY2dUGxc8f1mrhLt1AEpZkDjoO1YSAAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    17
    </td>
    <td align="left">
    ACT\_Mathematics \[variable, numeric\]
    </td>
    <td align="left">
    Mean (sd) : 20.5 (4.9) min \< med \< max: 1 \< 21 \< 36 IQR (CV) : 7
    (0.2)
    </td>
    <td align="left" style="vertical-align:middle">
    28 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAIBJREFUaN7t2bENgCAQQFFW0A3EDXT/3Ww8CghqlMTm/YpE75UGISW9abptzlH/nefYfrbBYDDYFVa+PCOwNQwYDAaDwWAwGAwGg8FqLH5AlxFYLJ5j5extCBYjMBgMBoPBYLBfseaW4wvWzPax9l73xHJ319nHypMyXGOfSnrTASVIDQqhiE/1AAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAw8Ku/1wAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMIH2B2sAAAAASUVORK5CYII=">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    18
    </td>
    <td align="left">
    ACT\_English \[variable, numeric\]
    </td>
    <td align="left">
    Mean (sd) : 21.2 (6) min \< med \< max: 1 \< 22 \< 36 IQR (CV) : 8
    (0.3)
    </td>
    <td align="left" style="vertical-align:middle">
    32 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAIxJREFUaN7t2cENgCAMhWFW0A2EDXT/3bzYHmwaTGnwwP9OJNAvgQMhpRQSydbNXiX+mu/Y9eQEAwMDAwMDA1sB05dUBtbEAAMDWwmTa+TIwGQABgYGBgY2GdMWYwomJWBgYGBgYL9jpvUzgplaH7OfdwOYziyKVfedEsCaWzsb021lYDrzPrOhFBLJDUydIAWMMpP5AAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAw8Ku/1wAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMIH2B2sAAAAASUVORK5CYII=">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    19
    </td>
    <td align="left">
    SWLS\_time1\_1 \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 3.1 (1.4) min \< med \< max: 1 \< 3 \< 5 IQR (CV) : 2
    (0.5)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    52
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    17.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    64
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    21.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    60
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    20.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    60
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    20.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    64
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    21.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAC0AAACABAMAAABttv2ZAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAFVJREFUSMdjYKA9UEIGioKCAlBxZWMkMCo+ZMVxxa8gGiBXXAkNKGBzDxCMig8vcVzxTqN0pSiA3T2j4sNMHFe8j6arUXFKxOmcrkbrwWEmjit+aQkAMGxVBEh/GMcAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDDwq7/XAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAwgfYHawAAAABJRU5ErkJggg==">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    20
    </td>
    <td align="left">
    SWLS\_time1\_2 \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 3.1 (1.1) min \< med \< max: 1 \< 3 \< 5 IQR (CV) : 2
    (0.4)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    25
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    8.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    59
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    19.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    118
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    39.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    61
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    20.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    37
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    12.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAEgAAACABAMAAABaeDKtAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAHFJREFUWMPt1LENwCAMRFFW8AiGDWD/3SKipAgC64ogMLpfv8YSRwh7Fu9U+j0olRoRkUsEvXEx+x3Fb9Z1b5mI6DgEDUHMZo+zqXtdUyYi8o+gIawdpwJfTyEiOg5BQxCzeeNUA6XBXUREDhD0xnfrAlqdvAjkmQvkAAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAw8Ku/1wAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMIH2B2sAAAAASUVORK5CYII=">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    21
    </td>
    <td align="left">
    SWLS\_time1\_3 \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 3.1 (1) min \< med \< max: 1 \< 3 \< 5 IQR (CV) : 2
    (0.3)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    10
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    3.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    98
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    32.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    68
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    22.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    107
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    35.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    17
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    5.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAEIAAACABAMAAABNWqJkAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAGxJREFUWMPt1LENgDAMRNGskBFiNgj77wZISCDAXBPFLv7Vr7F051LyxMyqk1Msa0cggoXuafUzUJiX2y3f6QhEIqGbHLSo9v4OjyAQCYVuctCirojvsP8HBCKN0E2et6j2K45bEIhYoXuaIRsHFaaXiaStMwAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    22
    </td>
    <td align="left">
    SWLS\_time1\_4 \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 3.1 (0.8) min \< med \< max: 1 \< 3 \< 5 IQR (CV) : 2
    (0.3)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    6
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    2.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    73
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    24.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    123
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    41.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    91
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    30.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    7
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    2.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAEoAAACABAMAAABejeKQAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAG9JREFUWMPt18sJgDAQRdG0MCU4dhD7700QF/4SHy7iMNy7PpuBPMFS4uZuvXY1VxQqk9Levdlo5bempxuXayhUfqWtw/oNWe2p5o3HKgqVUmnrCLPa1y/TdhsKlVpp6/hjtYqK+t+BQn1T2ruP2Aqg4sV6xuTDmAAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    23
    </td>
    <td align="left">
    SWLS\_time1\_5 \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 3.1 (0.9) min \< med \< max: 1 \< 3 \< 5 IQR (CV) : 2
    (0.3)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    1.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    99
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    33.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    65
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    21.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    128
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    42.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    1.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAE0AAACABAMAAAC8UfnpAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAGZJREFUWMPt1LENwCAMBVFGwBsEsgHZf7c0obECGEUKlrirX+PiOwT/5STd4uNOHG5DZ92HyBqXW+l7r/cKDocz78jLzlPD6XtxONz8jmTQXzvXHYO/VsPhdnbWHS3bOQ6H++w8dwOgjNVD4qpWswAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    24
    </td>
    <td align="left">
    SWLS\_time2\_1 \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 2.9 (1.4) min \< med \< max: 1 \< 3 \< 5 IQR (CV) : 2
    (0.5)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    57
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    19.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    73
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    24.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    58
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    19.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    57
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    19.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    55
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    18.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAADEAAACABAMAAABQJJz/AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAF5JREFUSMdjYKAfUEIBioKCggJQGWVjZDAqM7xlcKcDQQxAiYwSBlDA6jYQGJUZiTK4UwhNUyLOXGI0KjNCZXCnEFqmxMFVL4zKDAaZgamdBXG5zWhUZnjL4E4H9AAAUB5iP9OUQ5wAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDDwq7/XAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAwgfYHawAAAABJRU5ErkJggg==">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    25
    </td>
    <td align="left">
    SWLS\_time2\_2 \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 3 (1.1) min \< med \< max: 1 \< 3 \< 5 IQR (CV) : 2
    (0.4)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    29
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    9.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    70
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    23.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    116
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    38.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    51
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    17.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    34
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    11.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAEcAAACABAMAAACrc2kgAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAHJJREFUWMPt08EJgEAMRNEtwXSg2Q7W/nsTVHBBE+awYgzzz+8SyJQSMz1a5KnpRHXdIyL6I4J+XLzGI71lX3fViIhyIWgI43YHIfWarev6iIgSIGgIX47TQv11jYgoIYKGIF4vjtND1bqMiCg+gn48Whtu1rzVgZsqhgAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    26
    </td>
    <td align="left">
    SWLS\_time2\_3 \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 3 (1) min \< med \< max: 1 \< 3 \< 5 IQR (CV) : 2 (0.4)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    10
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    3.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    121
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    40.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    59
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    19.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    92
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    30.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    18
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    6.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAEkAAACABAMAAAC1ulmTAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAG9JREFUWMPt1LENgDAMRNGMgDcAs4HZfzeEBBFNjmsgkblfv8ZSLqWMm7tbu+lU6xZSUokU9+4N9oZy2FxvRElJ5VTcOrqvtvkz3QspqV8obh0G+2C1Vwu6sSYllVpx6+i02kd13BhSUmkU9+5HbAdYScXuZyFX9AAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    27
    </td>
    <td align="left">
    SWLS\_time2\_4 \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 3 (0.9) min \< med \< max: 1 \< 3 \< 5 IQR (CV) : 2
    (0.3)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    1.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    88
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    29.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    116
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    38.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    82
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    27.3%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    9
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    3.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAEcAAACABAMAAACrc2kgAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAGlJREFUWMPt07ENwCAMRFFGwBvEsEHYfzcaV8gJVyFj3a9fY8lXSsyaynfVUCciuhdBPy5yFDU3Xa4bXkREeRA0hBDjtJ7f6ywiogQIGkKEca7Iu+4lIkqEoCEcH6fuUR9ERNci6MejNQEvOL6cRxq/mwAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMPCrv9cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDCB9gdrAAAAAElFTkSuQmCC">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    28
    </td>
    <td align="left">
    SWLS\_time2\_5 \[numeric\]
    </td>
    <td align="left">
    Mean (sd) : 3 (0.9) min \< med \< max: 1 \< 3 \< 5 IQR (CV) : 2
    (0.3)
    </td>
    <td align="left" style="padding:0;vertical-align:middle">
    <table style="border-collapse:collapse;border:none;margin:0">
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    1
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    0.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    122
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    40.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    3
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    57
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    19.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    4
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    117
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    39.0%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    <tr style="background-color:transparent">
    <td style="padding:0 0 0 7px;margin:0;border:0" align="right">
    5
    </td>
    <td style="padding:0 2px;border:0;" align="left">
    :
    </td>
    <td style="padding:0 4px 0 6px;margin:0;border:0" align="right">
    2
    </td>
    <td style="padding:0;border:0" align="left">
    (
    </td>
    <td style="padding:0 2px;margin:0;border:0" align="right">
    0.7%
    </td>
    <td style="padding:0 4px 0 0;border:0" align="left">
    )
    </td>
    </tr>
    </table>
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAEoAAACABAMAAABejeKQAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqb39/f///+DdZCQAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAFxJREFUWMPt1bERABAMRmErZARswP67aTQcuVS4eK/+mhT/JYR3S6KGQn2s5LhKel3lqlVQKJfKto7bq40bNd6IQv2hbOsQvdO/Nq5vnEKhnCrbOi6sFoVCbdSLNaDFxzLBbTawAAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAw8Ku/1wAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMIH2B2sAAAAASUVORK5CYII=">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    29
    </td>
    <td align="left">
    exam\_1 \[variable, numeric\]
    </td>
    <td align="left">
    Mean (sd) : 58 (16.6) min \< med \< max: 0 \< 57 \< 100 IQR (CV) :
    23 (0.3)
    </td>
    <td align="left" style="vertical-align:middle">
    75 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAIxJREFUaN7t18ENgCAMQFFW0BFwA9l/NxPppQlBaBuj4f8LF/LSC42mRJa2x/Zc690Zx8rdCQYG5sDkUcZgR1XAvoH1Fu401hsQDAwM7B2sudasWHNAMDAwsBUw2aYxWFUKGBgYGBjYwpj+UXFiekAwsP9g8hBiMDmamPqY9WJKAQMDm8OyKhSzlsjSBbYSbHCfEUGvAAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAw8Ku/1wAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMIH2B2sAAAAASUVORK5CYII=">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    30
    </td>
    <td align="left">
    exam\_2 \[variable, numeric\]
    </td>
    <td align="left">
    Mean (sd) : 53.7 (18.8) min \< med \< max: 0 \< 54 \< 100 IQR (CV) :
    25.2 (0.4)
    </td>
    <td align="left" style="vertical-align:middle">
    81 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAI5JREFUaN7t1sENgCAMQFFWYAXcQPbfzRi4NGlQ25KI/n/h9tIDgaZElvKg0srX3cC2eraDgS2HFZETq+IAAwMDAwMDm4j1zzsGa0oFAwObi6lLtxVTBwQDAwN7GyY2Fi8mFLBgTH5STkwOCAYG9jtMe/zNmKaALYD1WxCD9QPsg9jguXiODRTzZM4SWToAbYeYj8yuvUEAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjAtMDUtMjBUMTE6MTY6NTgtMDU6MDDwq7/XAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAwgfYHawAAAABJRU5ErkJggg==">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    <tr>
    <td align="center">
    31
    </td>
    <td align="left">
    exam\_3 \[variable, numeric\]
    </td>
    <td align="left">
    Mean (sd) : 50.7 (18) min \< med \< max: 0 \< 51 \< 100 IQR (CV) :
    24 (0.4)
    </td>
    <td align="left" style="vertical-align:middle">
    79 distinct values
    </td>
    <td align="left" style="vertical-align:middle;padding:0;background-color:transparent">
    <img style="border:none;background-color:transparent;padding:0" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAJgAAABuBAMAAAApJ8cWAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqby8vL///8shn5hAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5AUUBhA63tkL3QAAAJhJREFUaN7t1s0JwCAMhmFXaEewG9T9dyvUXAIh/tIqvN/FizzEQ0xCID05ijljjnenHktvbjAwMLASJn/PHOzKChgY2CqYdPgcLCsJbBTzdr5mzCsQDAwMDAwMDKwGUyvjKKaU3TBzTevFzALBwMDAwL7FrBnXjVnKppieeIOYLnBBTF47B5MDbA3M6fB2zFF+xuKUBNKTByzAaaOZsiypAAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIwLTA1LTIwVDExOjE2OjU4LTA1OjAw8Ku/1wAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMC0wNS0yMFQxMToxNjo1OC0wNTowMIH2B2sAAAAASUVORK5CYII=">
    </td>
    <td align="center">
    0 (0%)
    </td>
    </tr>
    </tbody>
    </table>
    <p>
    Generated by
    <a href='https://github.com/dcomtois/summarytools'>summarytools</a>
    0.9.6 (<a href='https://www.r-project.org/'>R</a> version
    4.0.0)<br/>2020-05-20
    </p>
    </div>
    <!--/html_preserve-->

<!-- end list -->

``` r
# Needed to check my scales had decent Cronbach's alphas
dat %>%
  as.data.frame() %>%
  select(starts_with("SWLS_time2")) %>%
  psych::alpha()
```

    ## 
    ## Reliability analysis   
    ## Call: psych::alpha(x = .)
    ## 
    ##   raw_alpha std.alpha G6(smc) average_r S/N    ase mean sd median_r
    ##       0.96      0.97    0.97      0.86  30 0.0032    3  1     0.86
    ## 
    ##  lower alpha upper     95% confidence boundaries
    ## 0.95 0.96 0.97 
    ## 
    ##  Reliability if an item is dropped:
    ##              raw_alpha std.alpha G6(smc) average_r S/N alpha se   var.r med.r
    ## SWLS_time2_1      0.96      0.96    0.95      0.85  23   0.0041 0.00126  0.86
    ## SWLS_time2_2      0.95      0.96    0.95      0.86  24   0.0044 0.00090  0.86
    ## SWLS_time2_3      0.94      0.96    0.95      0.84  21   0.0046 0.00183  0.84
    ## SWLS_time2_4      0.95      0.96    0.95      0.85  24   0.0039 0.00221  0.87
    ## SWLS_time2_5      0.96      0.97    0.96      0.87  28   0.0035 0.00076  0.88
    ## 
    ##  Item statistics 
    ##                n raw.r std.r r.cor r.drop mean   sd
    ## SWLS_time2_1 300  0.95  0.94  0.93   0.92  2.9 1.39
    ## SWLS_time2_2 300  0.94  0.94  0.93   0.91  3.0 1.12
    ## SWLS_time2_3 300  0.96  0.96  0.95   0.93  3.0 1.04
    ## SWLS_time2_4 300  0.93  0.94  0.92   0.90  3.0 0.87
    ## SWLS_time2_5 300  0.91  0.92  0.89   0.87  3.0 0.92
    ## 
    ## Non missing response frequency for each item
    ##                 1    2    3    4    5 miss
    ## SWLS_time2_1 0.19 0.24 0.19 0.19 0.18    0
    ## SWLS_time2_2 0.10 0.23 0.39 0.17 0.11    0
    ## SWLS_time2_3 0.03 0.40 0.20 0.31 0.06    0
    ## SWLS_time2_4 0.02 0.29 0.39 0.27 0.03    0
    ## SWLS_time2_5 0.01 0.41 0.19 0.39 0.01    0

``` r
# Needed to check my ACT scores had the proper-ish relationships
cor(dat[, c(16:19)])
```

    ##                 ACT_Science ACT_Mathematics ACT_English SWLS_time1_1
    ## ACT_Science      1.00000000      0.79584740  0.70420149  -0.04557132
    ## ACT_Mathematics  0.79584740      1.00000000  0.85097543  -0.07409974
    ## ACT_English      0.70420149      0.85097543  1.00000000  -0.04436007
    ## SWLS_time1_1    -0.04557132     -0.07409974 -0.04436007   1.00000000

``` r
write.csv(dat, file = "dataset.csv", row.names = FALSE)
```
