# vael-ui-skills

An AI agent skill for [vael-ui](https://github.com/Mini-Sylar/vael-ui), an animation-agnostic Vue 3 component library with full Vue Vapor support.

Teaches an agent (Claude Code, or any agent supporting the shared [Agent Skills](https://vercel.com/docs/agent-resources/skills) format) how to install and use vael-ui correctly: the vdom/vapor split, auto-import setup, theming, the animation-agnostic contract (`motionCss`/`forceMount`/`beforeClose`), and the library's imperative composables.

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
