# DCMS Admin
The Dyalog Content Management System contains data related to user meeting presentations and the video library.

## How to add data from internal sources
Currently the source is in `U:/admin/dcms/DCMS.xlsx` in several sheets.

To push data to the live system, use:

```
Upload sheetname
```

Where `sheetname` is the name of one of the sheets in the workbook. If `'all'≡sheetname`, data is pushed in the following order:

1. Person
2. Event
3. Presentation
4. Presentation Material
5. Video

Updating data also triggers pulling data from YouTube to update video descriptions and thumbnails etc. in the video library as well as pushing content to the new website.

## Synchronise data with external resources
The `Refresh` function sends a request which causes the system to fetch video information from the YouTube API, update the search cache, and push content to the new website.
