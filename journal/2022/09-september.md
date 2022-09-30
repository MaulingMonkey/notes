## Friday September 30th, 2022


#### CURL
```sh
curl --etag-compare etag.txt --etag-save etag.txt -o saved-file https://example.com
curl -z saved-file -o saved-file https://example.com
```
<https://daniel.haxx.se/blog/2019/12/06/curl-speaks-etag/>
* `-z saved-file` - use `If-Modified-Since` header based on `saved-file` timestamps
* `-o saved-file` - output path
```rust

pub fn download_extract_assets() {
    let url = "https://thoseawesomeguys.com/prompts/Xelu_Free_Controller&Key_Prompts.zip";

    let xelu_dir = Path::new("thindx/examples/assets/xelu");
    std::fs::create_dir_all(xelu_dir).unwrap_or_else(|err| fatal!("unable to create directory {}: {}", xelu_dir.display(), err));

    let xelu_zip = Path::new("thindx/examples/assets/xelu/xelu.zip");
    if !xelu_zip.exists() {
        status!("Downloading", "{}", url);
        run(format!("curl {} --output {}", url, xelu_zip.display()));
    }

    let readme_txt = Path::new("thindx/examples/assets/xelu/Readme.txt");
    if !readme_txt.exists() {
        status!("Extracting", "{}", url);
        run_in(xelu_dir, "tar -xf xelu.zip");
    }
}
```
<https://github.com/MaulingMonkey/thindx/blob/3b671794597afd9e203984cfec522e7530b2e824/xtask/examples.rs#L86-L103>
