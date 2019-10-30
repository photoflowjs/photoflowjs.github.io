## PhotoFlowJs

PhotoFlow is an image layout library supporting \<image\> \<photo\> and \<video\> tags. PhotoFlow focuses on layouts that preserve the aspect ratio of images. Justified and Masonry are the currently supported layouts.

*Note: Consider this project "beta". Meaning the API can change at any point. Breaking API changes will happen in minor updates (e.g. 0.X.0 0.Y.0 may contain breaking api changes) until 1.0.*

### Justified Image Gallery

<div id="justified-container">
    <img src="https://images.unsplash.com/photo-1571847490051-491c12ff6540?ixlib=rb-1.2.1&amp;auto=format&amp;fit=crop&amp;w=1000&amp;q=80"/>
    <img src="https://images.unsplash.com/photo-1571680719972-f18bb57077cf?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=crop&amp;w=1000&amp;q=80"/>
    <img src="https://images.unsplash.com/photo-1571586100127-cdaef780fc61?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=crop&amp;w=1000&amp;q=80"/>
    <img src="https://images.unsplash.com/photo-1570171278960-d6c2b316f3b1?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=crop&amp;w=1000&amp;q=80"/>
    <img src="https://images.unsplash.com/photo-1569191086551-b3606745884f?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=crop&amp;w=1000&amp;q=80"/>
    <img src="https://images.unsplash.com/photo-1568860484667-b78d64242041?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=crop&amp;w=1000&amp;q=80"/>
    <img src="https://images.unsplash.com/photo-1569196769169-148d853ee706?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=crop&amp;w=1000&amp;q=80"/>
    <img src="https://images.unsplash.com/photo-1568021735466-efd8a4c435af?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=crop&amp;w=1000&amp;q=80"/>
    <img src="https://images.unsplash.com/photo-1568128979147-e03add161edb?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=crop&amp;w=1000&amp;q=80"/>
    <img src="https://images.unsplash.com/photo-1443926818681-717d074a57af?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=crop&amp;w=1000&amp;q=80"/>
</div>

### Basic Usage

```
<div id="gallery-wrapper">
    <img src="something.jpg" />
    <img src="another-picture.jpg" />
</div>
```
```
var imageWrapper = document.getElementById("gallery-wrapper");
Photoflow.init(imageWrapper);
```

### Options

```
{
    border?: number,
    margin?: number,
    debounceResizeWidth?: number,
    elementSelector?: (container: HTMLElement) => void,
    justified?: {
        targetRowHeight?: string|number,
        validRow?: (targetRowHeight: number, rowElementCount: number, currentRowHeight: number, totalElements: number) => boolean
    }
}
```
##### Border

Border wraps the entire gallery with a border.

Default = 0

##### Margin

Margin divides images in the gallery.

Default = 5px

##### DebounceResizeWidth

The change in width that causes a recalculation of the layout. This prevents flickering between 2 different layouts that have very similar goodness scores.

Default = 50px

##### ElementSelector

Used to determine which elements belong to the gallery.

Note: this must return an array, not a [NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList).

Default

```
function(container: HTMLElement): HTMLElement[] {
    var allSupportedTypes = Array.prototype.slice.call(container.querySelectorAll('img,video,picture'));
    var returnElements = [];
    for (var i = 0; i < allSupportedTypes.length; i++) {
        var currentElement = allSupportedTypes[i];
        if (!(currentElement.tagName === 'IMG' && currentElement.parentElement.tagName === 'PICTURE')) {
            returnElements.push(currentElement);
        }
    }
    return returnElements;
}
```

##### Justified.TargetRowHeight

The ideal height of each row in the justified gallery. Supports vw/vh values, representing percentages of the screen width/height respectively.

Default = 40vh

Examples: 40 (assumed pixels), "40px", "40vh"

##### Justified.ValidRow

Function used to determine if a row is valid.

Default:

```
function(targetRowHeight: number, rowElementCount: number, currentRowHeight: number, totalElements: number): boolean {
        if (totalElements < 5) {
            var minRowHeight = targetRowHeight / 4;
            var maxRowHeight = targetRowHeight * 3;
        } else {
            var minRowHeight = targetRowHeight * (0.6);
            var maxRowHeight = targetRowHeight * 2;
        }

        if (rowElementCount !== 1 && currentRowHeight < minRowHeight) {
            return false;
        }

        return currentRowHeight < maxRowHeight;
    }
}
```
### Events

Events can be set on the object returned by Photoflow.init

##### onready

Triggered the first time the gallery successfully renders.

##### onresize

Triggered on any resize, including resizes that don't trigger a recalculation of rows.
