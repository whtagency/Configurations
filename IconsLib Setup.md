# IconsLib Setup

## 1. `npm i svg-to-ts@8.6.1` ( tested version, you can use latest ).
## 2. Create new Folder (for example: 'ep-icons') =>
#### Create ep-icon Component,
#### Create ep-icon Module,
#### Create ep-icons-registry.service,
#### Create ep-icons.ts file, ALL IN ONE FOLDER

## 3. ep-icons-registry-service code:
<pre>
import { Injectable } from '@angular/core';

import { EpIcon } from './ep-icons';

@Injectable({
  providedIn: 'root',
})
export class EpIconsService {
  private registry = new Map<string, string>();

  public registerIcons(icons: EpIcon[]): void {
    icons.forEach((icon: EpIcon) => this.registry.set(icon.name, icon.data));
  }

  public getIcon(iconName: string): string | undefined {
    if (!this.registry.has(iconName)) {
      console.warn(
        `We could not find the dinosaur Icon with the name ${iconName}, did you add it to the Icon registry?`,
      );
    }
    return this.registry.get(iconName);
  }
}
</pre>

## 4. ep-icon component code:

<pre>
import { DOCUMENT } from '@angular/common';
import {
  ChangeDetectionStrategy,
  Component,
  ElementRef,
  Inject,
  Input,
  Optional,
} from '@angular/core';

import { epIcon } from './ep-icons';
import { EpIconsService } from './ep-icons.service';

@Component({
  selector: 'app-icon',
  template: `<ng-content></ng-content>`,
  styles: [
    ':host::ng-deep svg{display: flex; justify-content: center; align-items: center}',
  ],
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class EpIconComponent {
  private svgIcon: SVGElement | undefined;

  constructor(
    @Optional() @Inject(DOCUMENT) private document: Document,
    public element: ElementRef,
    private epIconsService: EpIconsService,
  ) {}

  @Input()
  set name(iconName: epIcon) {
    if (this.svgIcon) {
      this.element.nativeElement.removeChild(this.svgIcon);
    }
    const svgData = this.epIconsService.getIcon(iconName);

    if (!svgData) {
      return;
    }

    this.svgIcon = this.svgElementFromString(svgData);
    this.element.nativeElement.appendChild(this.svgIcon);
  }

  private svgElementFromString(svgContent: string): SVGElement {
    const div = this.document.createElement('DIV');
    div.innerHTML = svgContent;
    return (
      div.querySelector('svg') ||
      this.document.createElementNS('http://www.w3.org/2000/svg', 'path')
    );
  }
}
</pre>

## 5. To the ep-icons.ts file you must add

<pre>
export type epIcon = '';
export interface EpIcon {
  name: epIcon;
  data: string;
}
</pre>

## 6. Add new .svg-to-js.json file to your root level (on the same level with package.json).
## 7. Add this config to the .svg-to-js.json :
<pre>
{
  "verbose": true,
  "srcFiles": ["./src/assets/icons/*.svg"],
  "outputDirectory": "./src/ep-icons",
  "fileName": "ep-icons",
  "interfaceName": "EpIcon",
  "typeName": "epIcon",
  "prefix": "epIcons",
  "exportCompleteIconSet": true,
  "svgoConfig": {
    "plugins": ["cleanupAttrs"]
  }
}
</pre>

## 8. Add new script to the package.json file:
`"generate-icons": "svg-to-ts-constants --config .svg-to-ts.json"`

## How to use it:
In the parent module your component (ex. app-module) , you must add constructor
example:
<pre>
  constructor(private epIconsService: EpIconsService) {
    epIconsService.registerIcons(completeIconSet);
  }
</pre>

## How to create preview of all your icons:

#### You can create new page with custom url, for example: (‘/icons-preview’).

In this page you can import completeIconSet
variable from your library icons.ts file . This variable includes all your icons in the Object format,
you can render them in your html and create custom search with filter

### Example code with input in the HTML and searchControl for it:

<pre>
&lt;div class="header"&gt;
  &lt;h1&gt;EXPERT PAYS ICONS PACKAGE&lt;/h1&gt;
  &lt;div&gt;
    &lt;input
      [formControl]="searchControl"
      placeholder="Filter icons"
      type="text"
    /&gt;
  &lt;/div&gt;
&lt;/div&gt;
&lt;div class="icon-list"&gt;
  &lt;div *ngFor="let icon of icons" class="icon-card"&gt;
    &lt;div class="icon-card svg"&gt;
      &lt;app-icon [name]="icon.name"&gt;&lt;/app-icon&gt;
      &lt;p&gt;{{ icon.name }}&lt;/p&gt;
    &lt;/div&gt;
  &lt;/div&gt;
&lt;/div&gt;
</pre>

<pre>
body {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;

  font-family: 'SansSerif', sans-serif;
  font-size: 25px;
  font-style: normal;

  text-align: left;
  width: 100%;
  height: 150px;
  background-color: #264653;
  color: whitesmoke;
  border-radius: 5px;
}

input {
  background: whitesmoke;
  font-style: italic;
  font-size: 20px;
  padding-left: 15px;
  margin-left: 20px;
  height: 50px;
  width: 250px;
  border-width: 2px;
  border-style: inset;
  border-image: initial;
  outline: none;
  border-radius: 255px 25px 225px 25px/25px 225px 25px 255px;
}

.icon-list {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  width: 100%;
  margin-top: 50px;
}

.icon-card {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: #d0d0d0;
  min-height: 250px;
  min-width: 250px;
  margin: 20px;
  box-shadow: 0 2px 3px 0 #264653;
  border-radius: 5px;
  cursor: pointer;
}

.icon-card svg {
  height: 50px;
  width: 50px;
  margin-bottom: 10px;
}
</pre>

`this.searchControl.valueChanges.pipe(distinctUntilChanged()).subscribe((val: string) => { this.icons = completeIconSet.filter((el: EpIcon) => el.name.includes(val));`
