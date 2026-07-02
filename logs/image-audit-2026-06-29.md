# Image Audit Log - 2026-06-29

This audit was run against the Hugo site in `C:\Users\ADMIN\quickstart`.

## Confirmed issues fixed

- `content/2-Environmentsetup/2.2-createiamuserandattachpolicy/_index.md`
  Updated `021-saveaccesskey.png` to `021-createaccesskey.png`.
- `content/2-Environmentsetup/2.2-createiamuserandattachpolicy/_index.vi.md`
  Updated `021-saveaccesskey.png` to `021-createaccesskey.png`.
- `content/2-Environmentsetup/2.5-createdynamodbtableforinvoices/_index.md`
  Fixed six broken image paths that used `/2.5-createynamodb/` instead of `/2.5-createdynamodb/`.
- `content/3-AIpoweredinvoiceprocessing/3.4-createlambdafunction-fetch/_index.md`
  Updated the Save screenshot path to the existing file `009-clicksave (2).png`.
- `content/3-AIpoweredinvoiceprocessing/3.4-createlambdafunction-fetch/_index.vi.md`
  Updated the Save screenshot path to the existing file `009-clicksave (2).png`.
- `content/4-Deployingapigateway/4.1-creategetapigateway/_index.vi.md`
  Fixed `009-configuration.png` to the existing file `09-configuration.png`.
- `content/4-Deployingapigateway/4.2-createpostapigateway/_index.md`
  Fixed `007-createmethodd.png` to `007-createmethod.png`.
- `content/4-Deployingapigateway/4.2-createpostapigateway/_index.vi.md`
  Fixed `007-createmethodd.png` to `007-createmethod.png`.
- `content/5-Testwithpostman/5.2-Testgetallinvoices/_index.md`
  Fixed the final response screenshot from `Screenshot_15.png` to `Screenshot_13.png`.
- `content/5-Testwithpostman/5.2-Testgetallinvoices/_index.vi.md`
  Fixed the final response screenshot from `Screenshot_15.png` to `Screenshot_13.png`.

## Verified not actually broken

- `content/5-Testwithpostman/5.1-Testuploadfile/_index.md`
- `content/5-Testwithpostman/5.1-Testuploadfile/_index.vi.md`

The `image%281%29.png` style links are URL-encoded filenames for files such as `image(1).png`. These return `200 OK` under `hugo server` and do not need changes.

## Remaining unresolved issues

- `content/5-Testwithpostman/_index.md`
  References these missing assets:
  - `/images/arc-04.png`
  - `/images/5.fwd/001-fwd.png`
  - `/images/5.fwd/002-fwd.png`
  - `/images/5.fwd/003-fwd.png`
  - `/images/5.fwd/004-fwd.png`

Notes:
- This page is about Port Forwarding / Session Manager and appears unrelated to the current invoice-scanner flow.
- The entire `static/images/5.fwd/` folder is absent from the repository.
- `static/images/arc-logg.png` exists, but it is not a safe drop-in replacement for the missing Port Forwarding screenshots.

## Build verification

- `hugo --gc --minify` completed successfully on 2026-06-29.
- The only build warning observed was a Hugo deprecation warning for `.Site.Languages`.
