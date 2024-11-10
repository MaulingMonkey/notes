Chrome Browser Shenannigans



# Command Line

-   `chrome --new-window --app=http://google.com/`
-   `chrome --kiosk http://google.com/`
-   `... --window-size="2560,1000" --window-position="0,0"`
-   `... --user-data-dir="C:/Profiles/1"`



# References

-   <https://en.wikipedia.org/wiki/Chromium_Embedded_Framework>
    -   <https://bitbucket.org/chromiumembedded/cef/src/master/>
-   <https://wicg.github.io/manifest-incubations/index.html#dfn-borderless>
-   <https://github.com/sonkkeli/borderless/tree/main/demo-app>



# Data

-   `%LOCALAPPDATA%\Google\Chrome\User Data\Default\`
    -   `Bookmarks` - JSON
    -   `Favicons`  - Binary (`SQLite format 3`)

## Bookmarks

```json
{
    "checksum": "...",
    "roots": {
        "bookmark_bar": {
            "children": [
                {
                    "date_added": "13130000000000000",
                    "date_last_used": "0",
                    "guid": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
                    "id": "12345",
                    "name": "1x",
                    "type": "url",
                    "url": "javascript:(function f(){var v=document.getElementsByTagName(\"video\");for(var i=0;i\u003Cv.length;++i)v[i].playbackRate=1;})()"
                },
                ...
            ],
            "date_added": "13170000000000000",
            "date_last_used": "0",
            "date_modified": "13350000000000000",
            "guid": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
            "id": "1",
            "name": "Bookmarks bar",
            "type": "folder"
        },
        "other": {
            "children": [
            ],
            "date_added": "13170000000000000",
            "date_last_used": "0",
            "date_modified": "13330000000000000",
            "guid": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
            "id": "2",
            "name": "Other bookmarks",
            "type": "folder"
        },
        "synced": {
            "children": [
            ],
            "date_added": "13170000000000000",
            "date_last_used": "0",
            "date_modified": "0",
            "guid": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
            "id": "3",
            "name": "Mobile bookmarks",
            "type": "folder"
        }
    },
    "sync_metadata": "6 MB base64 blob?",
    "version": 1
}
```
