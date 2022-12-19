# Debatable design decisions

A list of design decisions that I may rectify in the future, in case you went really deep into this library and want to share your opinion on any of them. They are all very low priority, honestly, but I'd consider them if I ever intend to release a v0.1.0:
- Too much control over line height in the default renderer. This could be directly handled by the `Sizer` instead. Pros: it makes more sense, modularly speaking. Cons: it makes line height control slightly harder to reach and can complicate the `Sizer` interface. From personal experience, horizontal interspacing comes up more frequently than vertical insterspacing, and that's on the sizer already, so the first con doesn't seem too tragic.
- Text shaping probably should have its own interface or function. Providing methods for working with glyphs directly on eglyr is not terrible, but it doesn't feel quite right either. This isn't very relevant at the moment because `sfnt.Font` doesn't expose the relevant tables for text shaping yet, but... if we want to do text shaping kinda right, we probably need to step up the game in this area. The basic idea is `type Shaper interface { func(*etxt.Font, *bidi.Paragraph) []sfnt.GlyphIndex }`, passing paragraphs split with `golang.org/x/text/unicode/bidi`. Unclear about text direction, multiple fonts, language tags, single slice return being enough, the added bidi dependency, etc.
- `eglyr` seems fairly superfluous in a way, *specially* if we fix the previous point.
- Create an alias for `fixed.Int26_6`, like `Pix64ths`, `Pix64s` or `FractPix`. This will look more coherent in context and will avoid some redundant imports. This doesn't break anything actually and it could be nice, more explicit too. Methods are another thing to consider. In fact, this could lead to the elimination of the whole efixed package.
- Examples do not use `ebiten.DeviceScaleFactor()`. For correctness, they should, but maybe that would make the examples a bit too complicated. Also, only `LayoutF()` would be perfectly reliable, and that would be very messy (few examples are resizable though).
- Glyph mask rasterizer hacky identifying bits need some cleanup. While I made that part quite flexible with the best intentions in mind, I have never needed to modify any of it. Maybe it could simply... be simpler.