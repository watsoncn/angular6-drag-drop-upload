## Overview

An Angular 6 module for simple desktop file and folder drag and drop.

## Installation

```bash
npm install angular6-drag-drop-upload --save
```

## Usage

### Importing The 'ngx-file-drop' Module

```TypeScript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';
import { FileDropModule } from 'angular6-drag-drop-upload';


@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpClientModule,
    FileDropModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Enabling File Drag

```TypeScript
import { Component } from '@angular/core';
import { UploadEvent, UploadFile } from 'angular6-drag-drop-upload';

@Component({
  selector: 'demo-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {

  public files: UploadFile[] = [];

  public dropped(event: UploadEvent) {
    this.files = event.files;
    for (const droppedFile of event.files) {

      // Is it a file?
      if (droppedFile.fileEntry.isFile) {
        const fileEntry = droppedFile.fileEntry as FileSystemFileEntry;
        fileEntry.file((file: File) => {

          // Here you can access the real file
          console.log(droppedFile.relativePath, file);

          /**
          // You could upload it like this:
          const formData = new FormData()
          formData.append('logo', file, relativePath)

          // Headers
          const headers = new HttpHeaders({
            'security-token': 'mytoken'
          })

          this.http.post('https://mybackend.com/api/upload/sanitize-and-save-logo', formData, { headers: headers, responseType: 'blob' })
          .subscribe(data => {
            // Sanitized logo returned from backend
          })
          **/

        });
      } else {
        // It was a directory (empty directories are added, otherwise only files)
        const fileEntry = droppedFile.fileEntry as FileSystemDirectoryEntry;
        console.log(droppedFile.relativePath, fileEntry);
      }
    }
  }

  public fileOver(event){
    console.log(event);
  }

  public fileLeave(event){
    console.log(event);
  }
}
```

```HTML
<div class="center">
    <file-drop headertext="Drop files here" (onFileDrop)="dropped($event)"
    (onFileOver)="fileOver($event)" (onFileLeave)="fileLeave($event)">
        <span>optional content (don't set headertext then)</span>
    </file-drop>
    <div class="upload-table">
        <table class="table">
            <thead>
                <tr>
                    <th>Name</th>
                </tr>
            </thead>
            <tbody class="upload-name-style">
                <tr *ngFor="let item of files; let i=index">
                    <td><strong>{{ item.relativePath }}</strong></td>
                </tr>
            </tbody>
        </table>
    </div>
</div>
```

## Parameters

| Name          | Description                                      | Example                          |
| ------------- | ------------------------------------------------ | -------------------------------- |
| (onFileDrop)  | On drop function called after the files are read | (onFileDrop)="dropped($event)"   |
| (onFileOver)  | On drop over function                            | (onFileOver)="fileOver($event)"  |
| (onFileLeave) | On drop leave function                           | (onFileOver)="fileLeave($event)" |
| headertext    | Text to be displayed inside the drop box         | headertext="Drop files here"     |
| customstyle   | Custom style class name to be used               | customstyle="my-style"           |
| [disableIf]   | Conditionally disable the dropzone               | [disableIf]="condition"          |
