______________________________________________________________________________

### `GalleryCard.props.attr`

```html
<GalleryCard attr={...}>
    <div className='card' {...props.attr.container}>
        {props.background}
        <div className='card-content' {...props.attr.content}>
            {props.children}
        </div>
        <div className='banner' {...props.attr.banner}>
            {props.banner}
        </div>
    </div>
</GalleryCard>
```

______________________________________________________________________________

### Examples

```jsx
const SolidBackground = require('./SolidBackground').SolidBackground;
const GalleryCard = require('./GalleryCard').GalleryCard;

<div>
    <a href='#' className='link gallery-card-link'>
        <GalleryCard background={<SolidBackground backgroundColor='red'/>}>
            <header>Header</header>
            <section>Content</section>
        </GalleryCard>
    </a>
    <a href='#' className='link gallery-card-link'>
        <GalleryCard banner='Coming soon!'>
            <header>Header</header>
            <section>Content</section>
        </GalleryCard>
    </a>
    <a href='#' className='link gallery-card-link'>
        <GalleryCard banner='This is a very long banner. This is a very long banner. This is a very long banner. '>
            <header>Header</header>
            <section>This is very long content. This is very long content. This is very long content. This is very long content. This is very long content. This is very long content. This is very long content.</section>
        </GalleryCard>
    </a>
    <a href='#' className='link gallery-card-link disabled'>
        <GalleryCard>
            <header>Header</header>
            <section>Content</section>
        </GalleryCard>
    </a>

</div>
```