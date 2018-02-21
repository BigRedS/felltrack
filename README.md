# Felltrack scripts

Well, just the one for now.

The Felltrack UI's a bit of a data-dump, here's a script for parsing the CSV it
produces into some hopefully-useful information.

Download the entrant progess CSV file, and cat it into the script to get a 
summary of where the teams that are still out are, when you might next expect 
to hear from them and one or two useless bits of information:

    avi@t450:~/dev/felltrack$ cat entrantprogress.csv | ./progress 
          Team                                    Route           Last CP  Last Time Expected Time
    1     Spamalot                                50mile               14      22:40      00:47
    2     Monty Python                            50mile               13      20:58      22:55
    3     European Swallows                       50mile               13      20:44      22:41
    5     African Swallows                        50mile               14      21:58      00:05
    7     He's a lumberjack and he's okay         50mile               14      21:03      23:10
    10    He sleeps all night                     50mile               14      21:38      23:45
    26    and he works all dayd                   50km                 16      21:56      22:51
    27    I came here for an argument             50km                 16      21:56      22:51
    29    Incontinentia Buttocks                  50km                 17      22:41      23:17
    32    Romans Go Home                          50km                 16      22:04      22:59
    36    Judean Peoples Front                    50km                 17      22:27      23:03
    41    People's Front Of Judea                 50km                 18      21:53           
    43    The Cheesemakers                        50km                 14      20:26      22:33
    45    The knights of the round table          50km                 17      22:05      22:41
    46    A witch! a witch! A witch!              50km                 16      21:44      22:39
    50    Arthur, son of Uther Pendragon          50km                 16      22:12      23:07
    58    Uther Pendragon                         50km                 16      22:04      22:59
    
    
    Rearmost team:  CP13 European Swallows
    Frontmost team: CP18 People's Front of Judea
    
    Last-heard-from team:     CP17 @ 22:41 The Knighs of the round table
    Earliest-heard-from team: CP14 @ 20:26 The Cheesemakers
    
    Average time between CPs:
     0 ->  1 00:44
     1 ->  2 00:27
     2 ->  3 00:40
     3 ->  4 00:31
     4 ->  5 00:40
     5 ->  6 01:04
     6 ->  7 01:09
     7 ->  8 01:23
     9 -> 10 01:48
    10 -> 11 01:31
    11 -> 12 02:10
    12 -> 13 01:57
    13 -> 14 01:57
    14 -> 15 02:07
    15 -> 16 01:14
    16 -> 17 00:55
    17 -> 18 00:36

## Limitations

This was mostly written at about midnight, so there's a few things it doesn't 
get right:

* Teams with quote marks in their names - the Felltrack CSV file doesn't escape
  these quotes, and `Text::CSV` can't hack them so I just skip the lines; those
  teams happened to have finished by the time I was interested.

* Time estimates are between consecutively-numbered points only; walkers on the
  50k route go straight from CP8 to CP14; because 8 and 14 are not consecutive
  my crude averaging thing doesn't get any figures for that so can't guess. It
  will here produce numbers for 8->9.

* Those averages are just simple means; they ought really be weighted towards 
  more recent data, since later teams will be travelling at more similar speeds.

* "Team" figures are actually those of a single randomly-chosen not-retired 
  member of that team, so the numbers may change between runs.

* Scratch teams don't exist; they're shown split into each of the member's original
  teams (since the notion of a 'Team' is invented out of the entrant numbers),
  rather than being represented in the CSV)
