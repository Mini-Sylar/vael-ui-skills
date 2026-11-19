# vael-ui-skills

AI agent skills for two Vue 3 libraries by the same author, in the shared [Agent Skills](https://vercel.com/docs/agent-resources/skills) format (Claude Code, or any agent that supports it):

| Skill | For | Install separately |
| --- | --- | --- |
| [`vael-ui`](./skills/vael-ui) | [vael-ui](https://github.com/Mini-Sylar/vael-ui), a component library with full Vue Vapor support and one motion system you can take over part by part | yes |
| [`vael-motion`](./skills/vael-motion) | Motion in vael-ui apps: decides whether something should move, then picks the tool (the `motion` prop, `v-animate`, live values, gesture composables) and the values. Reads the `vael-ui` references | with `vael-ui` |
| [`vael-form`](./skills/vael-form) | [vael-form](https://github.com/Mini-Sylar/vael-form), Vue-first forms on Standard Schema (Zod, Valibot, ArkType…), with `v-model`, i18n and accessibility built in | yes |

The two libraries don't depend on each other: install the skill for the one you use, or both.

## vael-ui

Teaches an agent how to install and use vael-ui correctly: the vdom/vapor split, auto-import setup, theming, every component's props, slots, events and motion parts, the library's imperative composables, and the motion system: the `motion` prop, `v-animate`, springs, `enter`/`leave` hooks for GSAP or motion, and `ConfigProvider` motion styles, with rules for motion that feels right.

## vael-motion

The motion half of the library, as its own narrow skill so a motion task loads about 10 KB instead of the whole component guide. It runs a gate (how often it's seen, what it's for), casts the elements, walks a tool ladder (a component's `motion` prop, `v-animate`, `animate()`, live values with `mapRange`, `useDrag`, `useWheel`, `usePanZoom`, `useDragSelect`, hooks for your own animation library), gives the values and the split every gesture follows, and ends with what to feel-check. Install it next to `vael-ui`: it links to that skill's guides and recipes.

Version 2 targets vael-ui 0.5. It replaces the 0.4 animation hand-off (`motionCss`, `beforeClose(done)`, list events) and includes upgrade notes for code written against it.

## vael-form

Teaches an agent how to build forms with vael-form: setup (VDOM and Vapor, `formPlugin`, the Nuxt module, vue-i18n), `useForm` with a schema or plain `validate` functions, binding `model`, `attrs` and `errors` to plain inputs, vael-ui's `Field` and controls or other component libraries, when errors show, async `rules`, lists, steps and unions, submitting with server errors, FormData and TanStack Query, translated messages, SSR, the accessibility contract, testing, and a mapping for moving forms from vee-validate. It has the full API in `references/api.md`.

Version 1 targets vael-form 0.0.3.

## Install

**Claude Code:**

```
/plugin marketplace add Mini-Sylar/vael-ui-skills
/plugin install vael-ui@vael-ui-skills   # includes vael-ui and vael-motion
/plugin install vael-form@vael-ui-skills
```

Each skill is its own plugin in this marketplace; install one or both.

**Any Agent Skills-compatible agent (via [skill.sh](https://skill.sh)):**

```sh
npx skills add Mini-Sylar/vael-ui-skills                   # pick from both
npx skills add Mini-Sylar/vael-ui-skills --skill vael-ui
npx skills add Mini-Sylar/vael-ui-skills --skill vael-motion   # install next to vael-ui: it links to that skill's guides
npx skills add Mini-Sylar/vael-ui-skills --skill vael-form
```

## Updating

Each skill is versioned and released independently of its library. Updating a skill doesn't require a new library release, and a new library release doesn't require updating the skill unless the conventions it documents actually changed.
