<!-- LOVABLE:BEGIN -->
> [!IMPORTANT]
> This project is connected to [Lovable](https://lovable.dev). Avoid rewriting
> published git history — force pushing, or rebasing/amending/squashing commits
> that are already pushed — as it rewrites history on Lovable's side and the
> user will likely lose their project history.
>
> Commits you push to the connected branch sync back to Lovable and show up in
> the editor, so keep the branch in a working state.
<!-- LOVABLE:END -->

- Cinematic motion (cursor glow, card tilt, scroll scenes, active section) is driven by one rAF controller in src/components/cinematic/CinematicProvider.tsx writing CSS variables — keeps scrolling smooth without React re-renders.
- Modals rendered inside sections must be portaled to document.body, because section/card transforms break fixed positioning.
