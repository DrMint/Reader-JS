
<p align="center">
  <img src="https://r-entries.com/etuliens/img/Reader/icon.png" width="20%">
</p>


# Okuma-Reader
Online book reader written in JS with some key features like Japanese page order, double-page view and pre-caching on next pages.
The design is entirely cilent-side, the server simply hosts the images.

[Demo](https://okuma.r-entries.com/)

## Features

- Support for series with multiple chapters and volumes.
- Support for Japanese page ordering.
- Support for various screen ratio.
- Support for non-book materials (such as image sets)
- Support for vertical continuous scrolling (such as webtoons).
- Optional filters that simulate the visual experience of reading a real book (see Interface for images).
- A main page to showcase all available books.
- Page turn with arrow keys or by clicking/tapping on the edge of a page.
- Next/previous chapter buttons and chapter selection drop-down menu.
- Asynchronous pre-caching of the next pages (or next chapter first pages when at the end of a chapter).
- Single and double-page display with the ability to display the cover page as a single page.
- Automatically save the current page/chapter and selected options for when the user comes reading again.
- Config files per title and per volume.
- Animation when loading a page
- Multilingual support (open the lang folder for the list of available languages)
- Light-weight: under 200KB (including icons, filters, and textures).

## Interface

![](https://r-entries.com/etuliens/img/Reader/1.JPG)

At the top, there is information about the current book, the current chapter, a button to enter/exit fullscreen mode, and a button to open the settings menu. At the bottom, you will find the current page, a slider you can use to quickly skim through the book, and the total number of pages.

![](https://r-entries.com/etuliens/img/Reader/12.JPG)
When a book/manga has multiple chapters, a chapter selection menu is also present with arrows to go to the previous or next chapter. Please note that in Japanese ordering mode, the current page number is at the right, the total number of pages is at the left.

### Interface on mobile
<p align="center">
  <img src="https://r-entries.com/etuliens/img/Reader/3.JPG" width="50%">
</p>
On mobile, the interface is pretty much the same. The main difference is that the chapter selection menu will be absent to safe space.

### Options
![](https://r-entries.com/etuliens/img/Reader/2.JPG)
There is also a light mode. The user can tweak the reader settings in the side menu, such as single or double pages mode, turning on or off the filters and the current theme.

### Filters
The reader includes quite a few visual effects called filters. Their goal is to make the plain images look more like an actual book.

![](https://r-entries.com/etuliens/img/Reader/11.JPG)
With all filters turned off, this is just two plain images.

![](https://r-entries.com/etuliens/img/Reader/10.JPG)
The same pages with the filters turned on.
Here what each filter does:
 - Paper texture: adds some grain to the paper. The amount and type of grain depends on the book type (see the Config files section).
 - Book fold: adds a self-shadowed area in the middle of the book.
 - Realistic lighting: adds specularity to the paper.
 - Side pages: accurately simulates the fact that the previous and next pages are visible on the side of the book. (see the Side pages section).
 - Book shadow: adds a shadow to the bottom and side of the book to make it seems as if the book is laying on the surface.

### Side pages
To make the experience more realistic, I added a feature called side pages.
![](https://r-entries.com/etuliens/img/Reader/4.JPG)
Side pages are not shown on single pages such as the first page.

![](https://r-entries.com/etuliens/img/Reader/5.JPG)
At the beginning of the book, there is a lot of side pages on the right side. The amount of side pages shown actually depends on the number of remaining pages.

![](https://r-entries.com/etuliens/img/Reader/6.JPG)
With each page turned, the number of side pages on the left side will gradually increase and the opposite will occours on the right side.

## Library structure
- books/
	- a-example-book/
		- 1/
			- 1/
				- 1.jpg
				- 2.jpg
				- ...
			- 2/
				- 1.jpg
				- 2.jpg
				- ...
			- 3/
				- 1.jpg
				- ...
			- ...
			- config.json
		- 2/
			- ...
			- config.json
		- config.json
	- another-one/
		- 1/
			- ... 
			- config.json
		- config.json
	- config.json

A few rules and constraints:
- All images in a book must have the same extension
- The chapters must be numbered from 1 to n
- The pages of a chapter must be numbered from 1 to n
- If possible, all pages should have the same size (or at least the same ratio)
- A image should correspond to one page, images that have been combined as a double-page should be split

## Config files for titles and volumes
A config file must be provided for each title (series). Here's an example:

```
{
  "title": "My book's title",
  "bookType": "manga",
  "numVolumes": 1,
  "fileExtension": ".jpg",
  "japaneseOrder": true
}

```

numPages is the number of pages for each chapter. In this example, this book has 4 chapters.
The currently available types are "book", "manga", and "webtoons".

A config file for each volume must also be provided:

```
{
  "numPages": [38, 24, 40, 30],
  "fistPageSingle": true,
  "preferDoublePage": true,
  "allowDoublePage": true
}

```
fistPageSingle, preferDoublePage, and allowDoublePage doesn't work if the bookType is "webtoons"

## Installation

1. Clone the git: `git clone https://github.com/DrMint/Okuma-Reader.git`
2. Create library folder
3. Modify the js/constants.js with your website URLs

To update:
Simply use `git pull`.

If it fails with this kind of error:
```
error: Your local changes to the following files would be overwritten by merge:
        js/constants.js
Please commit your changes or stash them before you merge.
Aborting
```
It means that a new update changed constants.js. You will have to merge the differences yourself.
To do that:

1. Copy your modified js/constants.js: `cp js/constants.js ~/constants.js`
2. Undo changes to js/constants.js: `git checkout js/constants.js`
3. Update: `git pull`
4. Verify what's difference between js/constants.js and your copy at ~/constants.js
5. If there is a new line in js/constants.js, add it to your ~/constants.js
6. Restore your modified js/constants.js: `cp ~/constants.js js/constants.js`

## Prepare books

Convert PDFs into PNG images.

`-r600` means that the internal resolution is 600dpi

`-dDownScaleFactor=5` will downsampled the high resolution image into a more reasonable resolution.

`gs -dNOPAUSE -sDEVICE=png16m -r600 -dDownScaleFactor=5 -sOutputFile=export/%d.png name_of_my_book.pdf`

Then you can use a batch convertion tool to convert the images into JPGs, or even better, WebP.

## Things I want to add/change/improve (more or less in order of priority)
- Add a message/info pop-up when visiting the page for the first time with explanation on how to use it.
- When launching it for the first time, ask what "reading experience" the user wants: realistic (with the book filters) or simple (just the plain images)
- Add a book info pop-up where you can read the synopsis, learn about the author(s), status, date of release...
- Ability to give a name to each chapter.
- Ability to create bookmarks (especially for books)
- Ability for the user to create custom bookmarks
- Save the settings per book or globally.
- Give the user the ability to choose a custom color for the background.
- Add "reading mode" which is basically a blue-light filter. Later, add customizable filter layer.
- More flexibility per chapter.
- Page turning animation?!
- A Download page/chapter/book buttons? That would also open-up the idea of making it an offline Progressive Web App.
- The ability to display archives from the client's computer (without uploading it).

## License and attribution
This project uses [ironexdev](https://github.com/ironexdev)/[zoom](https://github.com/ironexdev/zoom) for handling pinch-zoom, pan, double-tap, zoom using the mouse wheel. It has been modified to allow single clicks (on the sides to go to the next/previous page and in the middle to toggle the top and bottom menus).
