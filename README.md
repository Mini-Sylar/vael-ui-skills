# vael-ui-skills

An AI agent skill for [vael-ui](https://github.com/Mini-Sylar/vael-ui), a Vue 3 component library with full Vue Vapor support and one motion system you can take over part by part.

Teaches an agent (Claude Code, or any agent supporting the shared [Agent Skills](https://vercel.com/docs/agent-resources/skills) format) how to install and use vael-ui correctly: the vdom/vapor split, auto-import setup, theming, every component's props, slots, events and motion parts, the library's imperative composables, and the motion system: the `motion` prop, `v-animate`, springs, `enter`/`leave` hooks for GSAP or motion, and `ConfigProvider` motion styles, with rules for motion that feels right.

Version 2 targets vael-ui 0.5. It replaces the 0.4 animation hand-off (`motionCss`, `beforeClose(done)`, list events) and includes upgrade notes for code written against it.

## Install

**Claude Code:**

```
/plugin marketplace add Mini-Sylar/vael-ui-skills
/plugin install vael-ui@vael-ui-skills
```

**Any Agent Skills-compatible agent (via [skill.sh](https://skill.sh)):**

```sh
npx skills add Mini-Sylar/vael-ui-skills
```

## Updating

This skill is versioned and released independently of the `vael-ui` package itself. Updating it doesn't require a new vael-ui release, and a new vael-ui release doesn't require updating this skill unless the conventions it documents actually changed.
