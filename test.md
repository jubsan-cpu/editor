The current umo editor is not paginated like microsoft word processor. you have to focus on the umo-page-content class becuase this is the page,umo-page-content is expanding type not paginated 

Requirments:
- The changes should be take place in the current container , Dont create new one meaning no new page , you work and mutate the current one.
- Each paginated page should be supporting every      
feature in the src/components/page-options.vue
- If one page filled - the text should flow to the next page 
- page should be synced with umo-zoomable-content
- Each page there should be a umo-page-node-header-content and umo-page-corner's also umo-page-node-footer-content.
- Dynamic margin should that is controlled by header/footer container height and for sides left/right margins are editor padding.
- Each of the page states are synced with each other meaning one page resize the other pages resizes with it,same goes for all the option in page-options
- Want live pagination inside the editor (content flows across multiple on-screen pages while editing), not just for print
- header/footer repeat on every page because of the page margin requirment the header footer container expands so the content inside shrinks
- Top/bottom margins are set as CSS vars and used as the heights of header/footer containers, so they don’t reduce the content frame; they reserve separate space above/below it.
- All existing page options in src/components/page-options.vue (size, orientation, margins, watermark, etc.) should apply per page and update all pages.
- Zoom behavior: Keep the existing zoom logic (in statusbar/index.vue) and have it scale all visible pages in the paginated view
- all options in src/components/page-options.vue apply globally to all pages uniformly
- Print/Export: printing/exporting reuse the on-screen paginated DOM and styling

Some answers:
- DOM based approch
- render multiple visual pages inside the existing umo-page-content container
- Top/bottom margins: exact CSS var names should be used
- left/right margins of ProseMirror content are the .umo-editor padding using --umo-page-margin-left/right, which are set from pageOptions.margin.
There’s also a separate outer container padding in page mode for visual gutter. 