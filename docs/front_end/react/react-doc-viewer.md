---
sgroup:
  title: 第三方组件
  order: 0
---

# react-doc-viewer

## 基础

```jsx
import DocViewer, { DocViewerRenderers } from '@cyntler/react-doc-viewer';

export default () => {
  const docs = [
    { uri: require('./assets/react-doc-viewer/csv-file.csv') },
    { uri: require('./assets/react-doc-viewer/gif-image.gif') },
    { uri: require('./assets/react-doc-viewer/pdf-file.pdf') },
    { uri: require('./assets/react-doc-viewer/pdf-multiple-pages-file.pdf') },
    { uri: require('./assets/react-doc-viewer/png-image.png') },
  ];

  return <DocViewer documents={docs} pluginRenderers={DocViewerRenderers} />;
};
```

## 初始化活动文档

```jsx
import DocViewer, { DocViewerRenderers } from '@cyntler/react-doc-viewer';

export default () => {
  const docs = [
    { uri: require('./assets/react-doc-viewer/csv-file.csv') },
    { uri: require('./assets/react-doc-viewer/gif-image.gif') },
    { uri: require('./assets/react-doc-viewer/pdf-file.pdf') },
    { uri: require('./assets/react-doc-viewer/pdf-multiple-pages-file.pdf') },
    { uri: require('./assets/react-doc-viewer/png-image.png') },
  ];

  return (
    <DocViewer
      documents={docs}
      initialActiveDocument={docs[1]}
      pluginRenderers={DocViewerRenderers}
    />
  );
};
```

## test

```jsx
import { useState } from 'react';
import DocViewer from '@cyntler/react-doc-viewer';

export default () => {
  const docs = [
    { uri: require('./assets/react-doc-viewer/csv-file.csv') },
    { uri: require('./assets/react-doc-viewer/gif-image.gif') },
    { uri: require('./assets/react-doc-viewer/pdf-file.pdf') },
    { uri: require('./assets/react-doc-viewer/pdf-multiple-pages-file.pdf') },
    { uri: require('./assets/react-doc-viewer/png-image.png') },
  ];
  const [activeDocument, setActiveDocument] = useState(docs[0]);

  const handleDocumentChange = (document) => {
    setActiveDocument(document);
  };

  return (
    <>
      <DocViewer
        documents={docs}
        activeDocument={activeDocument}
        onDocumentChange={handleDocumentChange}
      />
    </>
  );
};
```

## 上传文档

```jsx
import {useState} from "react";
import DocViewer, { DocViewerRenderers } from '@cyntler/react-doc-viewer';

export default () => {
  const [selectedDocs, setSelectedDocs] = useState<File[]>([]);

  return (
    <>
      <input
        type="file"
        accept=".pdf"
        multiple
        onChange={(el) =>
          el.target.files?.length &&
          setSelectedDocs(Array.from(el.target.files))
        }
      />
      <DocViewer
        documents={selectedDocs.map((file) => ({
          uri: window.URL.createObjectURL(file),
          fileName: file.name,
        }))}
        pluginRenderers={DocViewerRenderers}
      />
    </>
  );
};
```

## 自定义头部

```jsx
import DocViewer, { DocViewerRenderers } from '@cyntler/react-doc-viewer';

const MyHeader = (state, previousDocument, nextDocument) => {
  if (!state.currentDocument || state.config?.header?.disableFileName) {
    return null;
  }

  return (
    <>
      <div>{state.currentDocument.uri || ''}</div>
      <div>
        <button onClick={previousDocument} disabled={state.currentFileNo === 0}>
          Previous Document
        </button>
        <button
          onClick={nextDocument}
          disabled={state.currentFileNo >= state.documents.length - 1}
        >
          Next Document
        </button>
      </div>
    </>
  );
};

export default () => {
  const docs = [
    { uri: require('./assets/react-doc-viewer/csv-file.csv') },
    { uri: require('./assets/react-doc-viewer/gif-image.gif') },
    { uri: require('./assets/react-doc-viewer/pdf-file.pdf') },
    { uri: require('./assets/react-doc-viewer/pdf-multiple-pages-file.pdf') },
    { uri: require('./assets/react-doc-viewer/png-image.png') },
  ];

  return (
    <DocViewer
      documents={docs}
      pluginRenderers={DocViewerRenderers}
      config={{
        header: {
          overrideComponent: MyHeader,
        },
      }}
    />
  );
};
```
