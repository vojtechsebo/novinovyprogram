<script>
    import {calculateTickDimensions} from "./Layouter";
    import {toHumanTime} from "./utils";
    import {publisherMeta} from "./publisherMeta";
    export let item;
    export let article;
    export let publisherId;

    const {height, top} = calculateTickDimensions(article, item);
    const style = `height: ${height}px; top: ${top}px`;
    let link;
    const prefix = ['irozhlas', 'novinky', 'denik', 'seznamzpravy'].includes(publisherId)
        ?  'https://0yh5xmhkm0.execute-api.eu-west-1.amazonaws.com/prod?file='
        : 'https://s3-eu-west-1.amazonaws.com/lidovky-headlines/';
    const fileId = publisherMeta[publisherId].fileId;
    if(item.date.getTime() < 1558094783000) {
        link = `${prefix}${fileId}_${item.date.getTime()}.html`;
    } else {
        const datestamp = item.date.toISOString().replace(/[-:]/g, '').substr(0,15);
        link = `${prefix}${datestamp}_${fileId}.html`;
    }
    const articlesList = item.articles.map((article, index) => {
        return `${index + 1}) ${article.headline}`;
    });
    const title = `Hlavní články v ${toHumanTime(item.date)}:\n\n` +
        articlesList.join("\n") +
        `\n\nKliknutím zobrazíte zdrojový archivovaný soubor`;
</script>
<style>
.seen-tick {
    display: block;
    position: absolute;
    right: 0;
    border-top: 1px solid var(--full-color);
    border-bottom: 1px solid var(--full-color);
    background: var(--bg-color);
    width: 25px;
    opacity: 0;
    transition: opacity 0.2s;
}
.seen-tick:hover {
    opacity: 1;
}
.seen-tick .plus {
    position: absolute;
    top: 50%;
    left: 0;
    width: 100%;
    text-align: center;
    transform: translate(0, -50%);
    color: var(--full-color);
}

</style>
<a class="seen-tick" {style} {title} href="{link}" target="_blank">
    <span class="plus">+</span>
</a>
