**Time Check:** It is currently **1:06 AM (Dubai / +04)** on Wednesday, November 4, 2026 (2:36 AM IST).

  

### Why the Build Failed & Why VS Code Showed Errors

1. **`DemoApp.tsx` was placed in the wrong folder & `demo.css` was missing:** In your VS Code Explorer sidebar, `DemoApp.tsx` was created directly inside `tools/demo/DemoApp.tsx` instead of `tools/demo/src/DemoApp.tsx`, and `tools/demo/src/demo.css` was missing. Because the `tools/demo/src/` folder did not exist, `build.mjs` crashed with `ENOENT: no such file or directory, open '...\tools\demo\src\fixtures.generated.ts'`, and `tsconfig.json` reported that no files existed under `src/**/*`.
    
      
    
2. **Deleting `node_modules` and `fixtures.generated.ts` triggered VS Code TypeScript errors:** Step 4 of the previous script deleted `tools/demo/node_modules` and `fixtures.generated.ts`. Since both are already listed in `.gitignore`, Git ignores them anyway—keeping them on disk while only cleaning `assets/demo` removes all 55 TypeScript errors in VS Code while keeping `git status` 100% clean.
    
      
    
3. **Branch Name Typo:** Your current branch is named `feat/embed-gemara-reat-live-demo` (missing the `c` in `react`).
    
      
    
4. **Markdownlint & GitHub Actions Extension Warnings:**
    
      
    - The warnings in `sdk/index.md`, `index.md`, and `sdk/demo.md` (`MD047` missing trailing newline, `MD009` trailing spaces, `MD004` asterisk bullets, `MD022` blank lines around headings) are fixed in the script below.
        
          
        
    - The 1 warning in `deploy.yml` (`Value 'github-pages' is not valid` on line 132) is a harmless false positive from the VS Code GitHub Actions extension on forks that don't have a `github-pages` environment configured yet (that line is untouched original repo code).
        
          
        

### Run This Single PowerShell Script to Fix Everything Automatically

Copy and paste this **entire block** into your PowerShell terminal at `D:\Paarth\git\website`. It will create `tools/demo/src/`, move and write all files cleanly without copy-paste formatting bugs, run the build to verify everything compiles, and rename your branch:

  

PowerShell

````
$ErrorActionPreference = "Stop"

function Write-Utf8Lf([string]$RelativePath, [string]$Content) {
    $fullPath = Join-Path $PWD $RelativePath
    $dir = Split-Path -Parent $fullPath
    if (-not (Test-Path $dir)) {
        New-Item -ItemType Directory -Force -Path $dir | Out-Null
    }
    $normalized = $Content.Replace("`r`n", "`n").TrimEnd() + "`n"
    $utf8NoBom = New-Object System.Text.UTF8Encoding($false)
    [System.IO.File]::WriteAllText($fullPath, $normalized, $utf8NoBom)
}

# 1. Remove misplaced DemoApp.tsx in tools/demo/
if (Test-Path "tools/demo/DemoApp.tsx") {
    Remove-Item -Force "tools/demo/DemoApp.tsx"
}

# 2. Write tools/demo/package.json
Write-Utf8Lf "tools/demo/package.json" @'
{
  "name": "@gemara/website-demo",
  "version": "0.1.0",
  "private": true,
  "description": "Isolated @gemara/react live demo bundle for the Gemara documentation website",
  "type": "module",
  "scripts": {
    "build": "node build.mjs"
  },
  "dependencies": {
    "@gemara/react": "0.2.2",
    "react": "19.1.0",
    "react-dom": "19.1.0"
  },
  "devDependencies": {
    "@types/react": "19.1.0",
    "@types/react-dom": "19.1.0",
    "esbuild": "0.25.0",
    "typescript": "5.6.3",
    "yaml": "2.5.1"
  }
}
'@

# 3. Write tools/demo/tsconfig.json
Write-Utf8Lf "tools/demo/tsconfig.json" @'
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "jsx": "react-jsx",
    "strict": true,
    "skipLibCheck": true,
    "noEmit": true
  },
  "include": ["src/**/*"]
}
'@

# 4. Write tools/demo/src/DemoApp.tsx
Write-Utf8Lf "tools/demo/src/DemoApp.tsx" @'
// SPDX-License-Identifier: Apache-2.0
import React, { useState, useEffect, type ComponentType } from "react";
import { createRoot } from "react-dom/client";
import type { ArtifactType } from "@gemara/react";
import { GemaraProvider } from "@gemara/react/provider";
import { FormatTabs, CollapsibleGroup } from "@gemara/react/interactive";
import { ControlCatalog } from "@gemara/react/control-catalog";
import { GuidanceCatalog } from "@gemara/react/guidance-catalog";
import { ThreatCatalog } from "@gemara/react/threat-catalog";
import { CapabilityCatalog } from "@gemara/react/capability-catalog";
import { RiskCatalog } from "@gemara/react/risk-catalog";
import { Policy } from "@gemara/react/policy";
import { MappingDocument } from "@gemara/react/mapping-document";
import { PrincipleCatalog } from "@gemara/react/principle-catalog";
import { VectorCatalog } from "@gemara/react/vector-catalog";
import { Lexicon } from "@gemara/react/lexicon";
import { AuditLog } from "@gemara/react/audit-log";
import { EnforcementLog } from "@gemara/react/enforcement-log";
import { EvaluationLog } from "@gemara/react/evaluation-log";
import { FIXTURES, type DemoFixture } from "./fixtures.generated";

// eslint-disable-next-line @typescript-eslint/no-explicit-any
type Renderer = ComponentType<{ data: any; headingLevel?: number; idPrefix?: string }>;

const RENDERERS: Record<ArtifactType, Renderer> = {
  ControlCatalog,
  GuidanceCatalog,
  PrincipleCatalog,
  VectorCatalog,
  RiskCatalog,
  ThreatCatalog,
  CapabilityCatalog,
  Policy,
  Lexicon,
  MappingDocument,
  AuditLog,
  EnforcementLog,
  EvaluationLog,
};

function CollapsibleCatalogPreview({ fixture }: { fixture: DemoFixture }) {
  // eslint-disable-next-line @typescript-eslint/no-explicit-any
  const data = fixture.parsed as any;
  const groups: Array<{ id: string; title: string; description?: string }> = data.groups ?? [];

  if (fixture.type === "ControlCatalog" && groups.length > 0) {
    return (
      <GemaraProvider>
        <article data-gemara-artifact="ControlCatalog" data-gemara-id={data.metadata?.id}>
          <ControlCatalog.Header data={data} headingLevel={2} />
          <div className="demo-collapsible-list">
            {groups.map((group, idx) => (
              <CollapsibleGroup
                key={group.id}
                defaultOpen={idx === 0}
                summary={
                  <span className="demo-collapsible-summary">
                    <span className="demo-badge">{group.id}</span>
                    <strong>{group.title}</strong>
                  </span>
                }
              >
                <ControlCatalog.Group data={data} groupId={group.id} headingLevel={3} />
              </CollapsibleGroup>
            ))}
          </div>
        </article>
      </GemaraProvider>
    );
  }

  const RendererComponent = RENDERERS[fixture.type];
  return (
    <GemaraProvider>
      <RendererComponent data={fixture.parsed} headingLevel={2} idPrefix="demo-" />
    </GemaraProvider>
  );
}

function DemoApp() {
  const [selectedId, setSelectedId] = useState<string>(FIXTURES[0]?.id ?? "");
  const [collapsibleMode, setCollapsibleMode] = useState<boolean>(false);

  const activeFixture = FIXTURES.find((f: DemoFixture) => f.id === selectedId) ?? FIXTURES[0];

  useEffect(() => {
    function handleMessage(event: MessageEvent) {
      if (
        event.data &&
        typeof event.data === "object" &&
        event.data.type === "gemara-theme" &&
        (event.data.theme === "dark" || event.data.theme === "light")
      ) {
        document.documentElement.setAttribute("data-theme", event.data.theme);
      }
    }
    window.addEventListener("message", handleMessage);
    window.parent.postMessage({ type: "gemara-ready" }, "*");
    return () => window.removeEventListener("message", handleMessage);
  }, []);

  useEffect(() => {
    const sendHeight = () => {
      const height = document.documentElement.scrollHeight;
      window.parent.postMessage({ type: "gemara-resize", height }, "*");
    };
    sendHeight();
    const observer = new ResizeObserver(sendHeight);
    observer.observe(document.body);
    return () => observer.disconnect();
  }, [selectedId, collapsibleMode]);

  if (!activeFixture) {
    return <p>No demo fixtures available.</p>;
  }

  const RendererComponent = RENDERERS[activeFixture.type];
  const supportsCollapsible = activeFixture.type === "ControlCatalog";

  const previewNode =
    supportsCollapsible && collapsibleMode ? (
      <CollapsibleCatalogPreview fixture={activeFixture} />
    ) : (
      <RendererComponent data={activeFixture.parsed} headingLevel={2} idPrefix="preview-" />
    );

  return (
    <div className="demo-shell">
      <div className="demo-toolbar" role="region" aria-label="Demo controls">
        <div className="demo-toolbar-group">
          <label htmlFor="artifact-select" className="demo-label">
            Artifact Fixture
          </label>
          <select
            id="artifact-select"
            className="demo-select"
            value={activeFixture.id}
            onChange={(e: React.ChangeEvent<HTMLSelectElement>) => setSelectedId(e.target.value)}
          >
            {FIXTURES.map((f: DemoFixture) => (
              <option key={f.id} value={f.id}>
                {f.label} ({f.type} — {f.layer})
              </option>
            ))}
          </select>
        </div>

        <div className="demo-toolbar-meta">
          <span className="demo-badge demo-badge-layer">{activeFixture.layer}</span>
          <code className="demo-source-path">{activeFixture.sourcePath}</code>
          {supportsCollapsible && (
            <label className="demo-toggle">
              <input
                type="checkbox"
                checked={collapsibleMode}
                onChange={(e: React.ChangeEvent<HTMLInputElement>) => setCollapsibleMode(e.target.checked)}
              />
              <span>Interactive CollapsibleGroup</span>
            </label>
          )}
        </div>
      </div>

      <div className="demo-viewport">
        <GemaraProvider>
          <FormatTabs
            key={`${activeFixture.id}-${collapsibleMode}`}
            aria-label={`${activeFixture.type} formats`}
            tabs={[
              {
                id: "preview",
                label: "Rendered Preview (@gemara/react)",
                preview: previewNode,
              },
              {
                id: "yaml",
                label: "Raw YAML Source",
                language: "yaml",
                content: activeFixture.raw,
              },
            ]}
          />
        </GemaraProvider>
      </div>
    </div>
  );
}

const container = document.getElementById("root");
if (container) {
  createRoot(container).render(
    <React.StrictMode>
      <DemoApp />
    </React.StrictMode>
  );
}
'@

# 5. Write tools/demo/src/demo.css
Write-Utf8Lf "tools/demo/src/demo.css" @'
/* SPDX-License-Identifier: Apache-2.0 */
:root {
  --openssf-purple: #45208c;
  --openssf-purple-dark: #170d34;
  --openssf-purple-light: #604693;
  --openssf-purple-lighter: #7253ed;
  --openssf-green: #04ee5f;

  --color-bg-primary: #ffffff;
  --color-bg-secondary: #f6f8fa;
  --color-bg-tertiary: #f0f2f5;
  --color-border: #d0d7de;
  --color-border-light: #e7ecf0;
  --color-text-primary: #24292f;
  --color-text-secondary: #57606a;
  --color-text-tertiary: #6e7781;

  --radius-sm: 4px;
  --radius-md: 6px;
  --radius-lg: 8px;
}

[data-theme="dark"] {
  --openssf-purple: #a78bfa;
  --openssf-purple-dark: #c4b5fd;
  --openssf-purple-light: #8b5cf6;
  --openssf-purple-lighter: #a78bfa;
  --openssf-green: #34d399;

  --color-bg-primary: #1a1b23;
  --color-bg-secondary: #252732;
  --color-bg-tertiary: #2f3142;
  --color-border: #3d4054;
  --color-border-light: #2f3142;
  --color-text-primary: #e2e8f0;
  --color-text-secondary: #94a3b8;
  --color-text-tertiary: #64748b;
}

* {
  box-sizing: border-box;
}

html,
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  color: var(--color-text-primary);
  background-color: var(--color-bg-primary);
  line-height: 1.6;
  font-size: 15px;
}

.demo-shell {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 1rem;
}

.demo-toolbar {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
  padding: 0.875rem 1rem;
  background: var(--color-bg-secondary);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
}

.demo-toolbar-group {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex-wrap: wrap;
}

.demo-label {
  font-weight: 600;
  font-size: 0.875rem;
  color: var(--openssf-purple-dark);
}

.demo-select {
  padding: 0.45rem 0.75rem;
  border-radius: var(--radius-sm);
  border: 1px solid var(--color-border);
  background: var(--color-bg-primary);
  color: var(--color-text-primary);
  font-size: 0.9rem;
  font-weight: 500;
  cursor: pointer;
}

.demo-select:focus {
  outline: 2px solid var(--openssf-green);
  outline-offset: 1px;
}

.demo-toolbar-meta {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex-wrap: wrap;
}

.demo-source-path {
  font-size: 0.8rem;
  padding: 0.2rem 0.5rem;
  background: var(--color-bg-tertiary);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  color: var(--color-text-secondary);
}

.demo-toggle {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.85rem;
  font-weight: 500;
  cursor: pointer;
  user-select: none;
}

.demo-badge {
  display: inline-block;
  padding: 0.15rem 0.5rem;
  border-radius: var(--radius-sm);
  font-size: 0.75rem;
  font-weight: 600;
  background: rgba(114, 83, 237, 0.15);
  color: var(--openssf-purple-dark);
  border: 1px solid rgba(114, 83, 237, 0.3);
}

[data-gemara-part="format-tabs"] {
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  background: var(--color-bg-primary);
  overflow: hidden;
}

[data-gemara-part="format-tablist"] {
  display: flex;
  gap: 0.25rem;
  padding: 0.5rem 0.75rem 0;
  background: var(--color-bg-secondary);
  border-bottom: 1px solid var(--color-border);
}

[data-gemara-part="format-tab"] {
  appearance: none;
  background: transparent;
  border: 1px solid transparent;
  border-bottom: none;
  padding: 0.5rem 1rem;
  font-size: 0.875rem;
  font-weight: 600;
  color: var(--color-text-secondary);
  border-radius: var(--radius-sm) var(--radius-sm) 0 0;
  cursor: pointer;
  transition: all 0.15s ease;
}

[data-gemara-part="format-tab"]:hover {
  color: var(--openssf-purple);
  background: var(--color-bg-tertiary);
}

[data-gemara-part="format-tab"][data-gemara-selected] {
  background: var(--color-bg-primary);
  color: var(--openssf-purple-dark);
  border-color: var(--color-border);
  position: relative;
  top: 1px;
}

[data-gemara-part="format-panel"] {
  padding: 1.25rem;
}

[data-gemara-part="format-code"] {
  margin: 0;
  padding: 1rem;
  background: var(--color-bg-tertiary);
  border: 1px solid var(--color-border);
  border-left: 3px solid var(--openssf-purple);
  border-radius: var(--radius-md);
  overflow-x: auto;
  font-family: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, monospace;
  font-size: 0.85rem;
  line-height: 1.5;
  color: var(--color-text-primary);
}

[data-gemara-artifact] {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

[data-gemara-part="header"] {
  padding-bottom: 1rem;
  border-bottom: 2px solid var(--color-border-light);
}

[data-gemara-part="title"] {
  margin: 0 0 0.5rem 0;
  color: var(--openssf-purple-dark);
  font-size: 1.5rem;
}

[data-gemara-part="meta"] {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 0.5rem 1rem;
  margin: 0.75rem 0 0;
  padding: 0.75rem 1rem;
  background: var(--color-bg-secondary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-sm);
  font-size: 0.85rem;
}

[data-gemara-part="meta"] dt {
  font-weight: 700;
  color: var(--color-text-secondary);
  text-transform: uppercase;
  font-size: 0.72rem;
  letter-spacing: 0.04em;
}

[data-gemara-part="meta"] dd {
  margin: 0 0 0.35rem 0;
  color: var(--color-text-primary);
}

[data-gemara-part="group"] {
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: 1rem 1.25rem;
  margin-bottom: 1rem;
  background: var(--color-bg-secondary);
}

[data-gemara-part="control-list"],
[data-gemara-part="guideline-list"],
[data-gemara-part="threat-list"],
[data-gemara-part="capability-list"],
[data-gemara-part="risk-list"],
[data-gemara-part="mapping-list"] {
  list-style: none;
  padding: 0;
  margin: 0.75rem 0 0;
  display: flex;
  flex-direction: column;
  gap: 0.875rem;
}

[data-gemara-part="control"],
[data-gemara-part="guideline"],
[data-gemara-part="threat"],
[data-gemara-part="capability"],
[data-gemara-part="risk"],
[data-gemara-part="mapping"],
[data-gemara-part="constraint"],
[data-gemara-part="assessment-plan"] {
  background: var(--color-bg-primary);
  border: 1px solid var(--color-border);
  border-left: 4px solid var(--openssf-purple);
  border-radius: var(--radius-sm);
  padding: 0.875rem 1rem;
}

[data-gemara-part="control-id"],
[data-gemara-part="guideline-id"],
[data-gemara-part="threat-id"],
[data-gemara-part="capability-id"],
[data-gemara-part="risk-id"],
[data-gemara-part="statement-id"],
[data-gemara-part="requirement-id"] {
  display: inline-block;
  font-family: "SFMono-Regular", Consolas, monospace;
  font-size: 0.78rem;
  font-weight: 700;
  padding: 0.1rem 0.45rem;
  margin-right: 0.5rem;
  border-radius: var(--radius-sm);
  background: rgba(114, 83, 237, 0.12);
  color: var(--openssf-purple-dark);
}

[data-gemara-part="requirements"],
[data-gemara-part="statements"],
[data-gemara-part="recommendations"] {
  margin-top: 0.75rem;
  padding-left: 1.25rem;
}

[data-gemara-part="requirement"],
[data-gemara-part="statement"] {
  margin-bottom: 0.5rem;
  padding: 0.5rem 0.75rem;
  background: var(--color-bg-secondary);
  border-radius: var(--radius-sm);
  list-style: none;
}

[data-gemara-part="references"],
[data-gemara-part="document-references"] {
  margin-top: 0.75rem;
  padding: 0.5rem 0.75rem;
  background: var(--color-bg-tertiary);
  border: 1px solid var(--color-border-light);
  border-radius: var(--radius-sm);
  font-size: 0.85rem;
}

[data-gemara-part="references-summary"],
[data-gemara-part="document-references-summary"] {
  cursor: pointer;
  font-weight: 600;
  color: var(--openssf-purple);
}

[data-gemara-prose] {
  white-space: pre-wrap;
  margin: 0.4rem 0;
}

.demo-collapsible-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

[data-gemara-part="collapsible"] {
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  background: var(--color-bg-secondary);
  overflow: hidden;
}

[data-gemara-part="collapsible-trigger"] {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.75rem 1rem;
  background: var(--color-bg-secondary);
  border: none;
  color: var(--color-text-primary);
  font-size: 0.95rem;
  cursor: pointer;
  text-align: left;
}

[data-gemara-part="collapsible-trigger"]:hover {
  background: var(--color-bg-tertiary);
}

.demo-collapsible-summary {
  display: inline-flex;
  align-items: center;
  gap: 0.6rem;
}

[data-gemara-part="collapsible-content"] {
  padding: 0.75rem 1rem;
  border-top: 1px solid var(--color-border);
  background: var(--color-bg-primary);
}

a {
  color: var(--openssf-purple);
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
'@

# 6. Write tools/demo/build.mjs (with mkdirSync for src/)
Write-Utf8Lf "tools/demo/build.mjs" @'
// SPDX-License-Identifier: Apache-2.0
import { readFileSync, writeFileSync, mkdirSync, copyFileSync } from "node:fs";
import { resolve, dirname } from "node:path";
import { fileURLToPath } from "node:url";
import { parse as parseYaml } from "yaml";
import * as esbuild from "esbuild";
import { detectArtifactType } from "@gemara/react";

const __dirname = dirname(fileURLToPath(import.meta.url));
const repoRoot = resolve(__dirname, "..", "..");
const srcDir = resolve(__dirname, "src");
const outDir = resolve(repoRoot, "assets", "demo");

const SOURCES = [
  {
    id: "control-catalog",
    label: "Container Security Control Catalog",
    layer: "Layer 2",
    relPath: "tutorials/controls/control-catalog.yaml",
  },
  {
    id: "guidance-catalog",
    label: "Cryptographic Key Management Guidance",
    layer: "Layer 1",
    relPath: "tutorials/guidance/guidance-example.yaml",
  },
  {
    id: "threat-catalog",
    label: "Container Security Threat Catalog",
    layer: "Layer 2",
    relPath: "tutorials/controls/threat-catalog.yaml",
  },
  {
    id: "capability-catalog",
    label: "Container Security Capabilities",
    layer: "Layer 2",
    relPath: "tutorials/controls/capabilities.yaml",
  },
  {
    id: "risk-catalog",
    label: "Enterprise Risk Catalog",
    layer: "Layer 2",
    relPath: "tutorials/policy/risk-catalog-example.yaml",
  },
  {
    id: "policy",
    label: "Organizational Security Policy",
    layer: "Layer 3",
    relPath: "tutorials/policy/policy-example.yaml",
  },
  {
    id: "mapping-document",
    label: "Cross-Framework Mapping Document",
    layer: "Layer 2",
    relPath: "tutorials/mapping/mapping-document.yaml",
  },
];

const loadedFixtures = SOURCES.map((src) => {
  const fullPath = resolve(repoRoot, src.relPath);
  const raw = readFileSync(fullPath, "utf8");
  const parsed = parseYaml(raw);
  const type = detectArtifactType(parsed);
  if (!type) {
    throw new Error(`Build verification failed: ${src.relPath} is not a valid Gemara artifact`);
  }
  return {
    id: src.id,
    label: src.label,
    layer: src.layer,
    sourcePath: src.relPath,
    type,
    raw,
    parsed,
  };
});

const generatedTs = `// AUTO-GENERATED AT BUILD TIME — DO NOT COMMIT
import type { ArtifactType } from "@gemara/react";

export interface DemoFixture {
  id: string;
  label: string;
  layer: string;
  sourcePath: string;
  type: ArtifactType;
  raw: string;
  parsed: unknown;
}

export const FIXTURES: DemoFixture[] = ${JSON.stringify(loadedFixtures, null, 2)};
`;

mkdirSync(srcDir, { recursive: true });
writeFileSync(resolve(srcDir, "fixtures.generated.ts"), generatedTs, "utf8");

mkdirSync(outDir, { recursive: true });

await esbuild.build({
  entryPoints: [resolve(srcDir, "DemoApp.tsx")],
  bundle: true,
  minify: true,
  format: "esm",
  target: ["es2020"],
  outfile: resolve(outDir, "bundle.js"),
  define: {
    "process.env.NODE_ENV": '"production"',
  },
});

copyFileSync(resolve(srcDir, "demo.css"), resolve(outDir, "bundle.css"));

const frameHtml = `<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta http-equiv="Content-Security-Policy" content="default-src 'none'; script-src 'self'; style-src 'self'; img-src 'self' data:; font-src 'self'; connect-src 'none'; form-action 'none'; base-uri 'none';" />
  <title>Gemara React Live Demo Frame</title>
  <link rel="stylesheet" href="./bundle.css" />
</head>
<body>
  <div id="root"></div>
  <noscript>
    <p>JavaScript is required to run the interactive @gemara/react renderer demo.</p>
  </noscript>
  <script type="module" src="./bundle.js"></script>
</body>
</html>
`;

writeFileSync(resolve(outDir, "frame.html"), frameHtml, "utf8");
console.log(`  >  Built @gemara/react demo bundle (${loadedFixtures.length} verified artifacts) -> assets/demo/`);
'@

# 7. Write sdk/demo.md (with markdownlint cleanups)
Write-Utf8Lf "sdk/demo.md" @'
---
layout: page
title: React SDK Live Demo
---

<!-- markdownlint-disable MD033 -->

[`@gemara/react`](https://github.com/gemaraproj/gemara-react) provides headless, server-component-safe React renderers and interactive primitives for [Gemara](https://github.com/gemaraproj/gemara) artifacts. Every renderer emits semantic HTML with stable `data-gemara-*` attributes so applications can bring their own styling.

Use the interactive sandbox below to inspect live Gemara artifacts from the [Tutorials](../tutorials/) directory rendered with `@gemara/react`.

<div class="demo-iframe-container" style="margin: 1.5rem 0; border: 2px solid var(--color-border); border-radius: var(--radius-md); overflow: hidden; background: var(--color-bg-secondary); box-shadow: var(--shadow-sm);">
  <iframe
    id="gemara-demo-frame"
    src="{{ '/assets/demo/frame.html' | relative_url }}"
    title="Gemara React Live Artifact Renderer Demo"
    sandbox="allow-scripts"
    loading="lazy"
    style="width: 100%; min-height: 680px; border: 0; display: block;"
  ></iframe>
</div>

<noscript>
  <blockquote>
    <strong>Note:</strong> JavaScript is disabled in your browser. View the raw YAML fixtures directly in the <a href="../tutorials/">Tutorials</a> section or explore the <a href="https://github.com/gemaraproj/gemara-react">@gemara/react repository</a>.
  </blockquote>
</noscript>

<script>
(function () {
  var iframe = document.getElementById("gemara-demo-frame");
  if (!iframe) return;

  function syncTheme() {
    var theme = document.documentElement.getAttribute("data-theme") === "dark" ? "dark" : "light";
    if (iframe.contentWindow) {
      iframe.contentWindow.postMessage({ type: "gemara-theme", theme: theme }, "*");
    }
  }

  iframe.addEventListener("load", syncTheme);

  window.addEventListener("message", function (event) {
    if (!event.data || typeof event.data !== "object") return;
    if (event.data.type === "gemara-ready") {
      syncTheme();
    } else if (event.data.type === "gemara-resize" && typeof event.data.height === "number") {
      iframe.style.height = Math.max(480, event.data.height + 16) + "px";
    }
  });

  var observer = new MutationObserver(syncTheme);
  observer.observe(document.documentElement, {
    attributes: true,
    attributeFilter: ["data-theme"]
  });
})();
</script>

## What This Demo Showcases

- **Headless Artifact Renderers:** Renders `ControlCatalog`, `GuidanceCatalog`, `ThreatCatalog`, `CapabilityCatalog`, `RiskCatalog`, `Policy`, and `MappingDocument` artifacts validated at build time with `detectArtifactType`.
- **Interactive Primitives (`@gemara/react/interactive`):**
  - `FormatTabs` switches between the live rendered preview and the raw YAML source using the WAI-ARIA tabs pattern.
  - `CollapsibleGroup` toggles collapsible control groups on `ControlCatalog` artifacts.
- **Strict Sandbox Isolation:** Runs inside an isolated `<iframe sandbox="allow-scripts">` with a locked-down Content Security Policy (`default-src 'none'`).

## Quick Start

Install `@gemara/react` from npm:

```bash
npm install @gemara/react
```

Render a `ControlCatalog` inside your React or Astro application:

  

TypeScript

```
import { ControlCatalog } from "@gemara/react/control-catalog";
import { GemaraProvider } from "@gemara/react/provider";
import { isControlCatalog } from "@gemara/react";

export function CatalogView({ data }: { data: unknown }) {
  if (!isControlCatalog(data)) {
    throw new Error("Invalid ControlCatalog artifact");
  }

  return (
    <GemaraProvider>
      <ControlCatalog data="{data}" headingLevel="{2}"/>
    </GemaraProvider>
  );
}
```

## Related Resources

- **[@gemara/react Repository](https://github.com/gemaraproj/gemara-react)** — Source code, `data-gemara-*` styling contract, and API documentation
    
      
    
- **[SDK Overview](https://www.google.com/search?q=./index.html)** — Versioning and architecture across Gemara SDKs
    
      
    
- **[Tutorials](https://www.google.com/search?q=../tutorials/)** — Step-by-step guides for the YAML artifacts rendered in this demo '@
    
      
    

# 8. Write sdk/index.md (fixing MD004, MD022, MD047)

## Write-Utf8Lf "sdk/index.md" @'

## layout: page title: SDKs

Language-specific SDKs provide programmatic access to Gemara documents. SDK types are generated from CUE schemas, ensuring consistency between validation and programmatic access.

  

## Available SDKs

- [Go SDK](https://www.google.com/search?q=./go-sdk.html) — Type-safe Go APIs for reading, writing, and manipulating Gemara documents
    
      
    
- [React SDK Live Demo (`@gemara/react`)](https://www.google.com/search?q=./demo.html) — Headless React renderers and interactive components for Gemara artifacts
    
      
    

## Versioning and Maintenance

Project release deliverables are divided into the **Core Specification** and language-specific **SDKs**.

  

- **Core Specification Release:** The CUE schemas are versioned and released as the core specification. These schemas implement the Model, which is published separately.
    
      
    
- **SDK Releases:** Language-specific implementations that support programmatic access to Gemara documents. SDK types are generated from the CUE schemas.
    
      
    

Each maintains its own independent [SemVer](https://semver.org/) lifecycle.

  

### SDK Release Versioning

SDK releases are versioned independently to allow for rapid iteration on tooling, bug fixes, and utilities without requiring core specification version bumps.

  

- The CUE schemas in the core specification release are the source of truth for validation.
    
      
    
- SDKs must explicitly document which core specification release version they support.
    
      
    
- When a new core specification release is published, SDKs regenerate their types from the updated schemas and release new versions that support the updated specification.
    
      
    

## Relationship to Other Components

### [The Model](https://www.google.com/search?q=../model)

Provides the conceptual foundation. SDK types correspond to elements in the model.

  

### [The Schemas](https://www.google.com/search?q=../schema/)

SDK types are generated from the CUE schemas. The schemas are the source of truth for data structures. '@

  

# 9. Write index.md (fixing MD009, MD012, MD047)

## Write-Utf8Lf "index.md" @'

## layout: home title: Home

# Gemara (Juh-MAH-ruh)

**GRC Engineering Model for Automated Risk Assessment**

  

Gemara provides a logical model to describe the categories of compliance activities, how they interact, and the schemas to enable automated interoperability between them.

  

In order to better facilitate cross-functional communication, the Gemara Model seeks to outline the categorical layers of activities related to automated governance.

  

## The Three Components

Gemara delivers three core components that work together to support automated GRC:

  

## Quick Start

Choose your starting point based on your needs:

  

- **Understanding GRC structure?** Start with **[The Model](https://www.google.com/search?q=./model)** component
    
      
    
- **Validating documents?** Use **[The Schemas](https://www.google.com/search?q=./schema/)** component
    
      
    
- **Building tools?** Jump to **[The SDKs](https://www.google.com/search?q=./sdk/)** component
    
      
    
- **Exploring rendered artifacts?** Try the **[React SDK Live Demo](https://www.google.com/search?q=./sdk/demo.html)**
    
      
    

## Real-World Usage

Gemara is being used today in production environments:

  

- **[FINOS Common Cloud Controls](https://www.finos.org/common-cloud-controls-project)** - Layer 2 controls for cloud environments
    
      
    
- **[Open Source Project Security Baseline](https://baseline.openssf.org/)** - Layer 2 security baseline for open source projects
    
      
    
- **[Privateer](https://github.com/privateerproj/privateer)** - Layer 5 evaluation framework with plugins like the [OSPS Baseline Plugin](https://github.com/revanite-io/pvtr-github-repo) '@
    
      
    

# 10. Write Makefile with exact TABs and LF line endings

Write-Utf8Lf "Makefile" @'

  

# Makefile for the Gemara website (gemara.openssf.org)

# The site content lives at the repository root. Schema reference pages,

# the definitions table, and term cross-links are GENERATED from the Gemara

# specification's OpenAPI projection (openapi.yaml), which the spec repo

# (github.com/gemaraproj/gemara) publishes as a release asset. The markdown

# tooling that renders it lives in this repo under tools/.

# openapi.yaml acquisition, in order of precedence:

# GEMARA_OPENAPI=/path/to/openapi.yaml use a pre-generated file

# GEMARA_DIR=../gemara generate from a local spec checkout

# (runs its cue2openapi command)

# GEMARA_REF=v1.2.3 (default: latest) download the release asset

GEMARA_REPO ?= https://github.com/gemaraproj/gemara GEMARA_REF ?= latest

  

SITE_ABS := $(abspath .) TOOLS_DIR := tools GENERATED_DIR := generated OPENAPI_YAML := $(GENERATED_DIR)/openapi.yaml SPEC_MD_DIR := $(GENERATED_DIR)/spec SCHEMA_DIR := schema SCHEMA_NAV := schema-nav.yml

  

.PHONY: all fetch-openapi genmd gendocs build-demo serve build test-links cleanup cleanup-links check-jekyll deps

  

all: gendocs test-links cleanup

  

deps: @echo " > Running bundle install..." @bundle install

  

check-jekyll: @if ! bundle exec jekyll -v >/dev/null 2>&1; then

  

echo "ERROR: Jekyll not available. Install dependencies: make deps";

  

exit 1;

  

fi

  

# File target: if generated/openapi.yaml already exists (e.g. CI downloaded

# or generated it beforehand), acquisition is skipped entirely.

$(OPENAPI_YAML): @mkdir -p $(GENERATED_DIR) @if [ -n "$(GEMARA_OPENAPI)" ]; then \ echo " > Using local OpenAPI file $(GEMARA_OPENAPI) ..."; \ cp "$(GEMARA_OPENAPI)" "$(OPENAPI_YAML)";

  

elif [ -n "$(GEMARA_DIR)" ]; then \ echo " > Generating OpenAPI from local spec checkout $(GEMARA_DIR) ..."; \ cd "$(abspath $(GEMARA_DIR))/cmd" && go run . cue2openapi \ --schema .. \ --output $(SITE_ABS)/$(OPENAPI_YAML); \ else \ if [ "$(GEMARA_REF)" = "latest" ]; then \ url="$(GEMARA_REPO)/releases/latest/download/openapi.yaml";

  

else

  

url="$(GEMARA_REPO)/releases/download/$(GEMARA_REF)/openapi.yaml";

  

fi;

  

echo " > Downloading $$url ...";

  

curl --fail --silent --show-error --location "$$url" --output "$(OPENAPI_YAML)" || { \ rm -f "$(OPENAPI_YAML)";

  

echo "ERROR: could not download openapi.yaml for spec ref '$(GEMARA_REF)'.";

  

echo "Releases before the asset existed can be built from a checkout instead:";

  

echo " make gendocs GEMARA_DIR=/path/to/gemara";

  

exit 1;

  

};

  

fi

  

fetch-openapi: $(OPENAPI_YAML)

  

genmd: fetch-openapi @echo " > Generating markdown from OpenAPI ..." @mkdir -p $(SPEC_MD_DIR) @cd $(TOOLS_DIR) && go run . openapi2md

  

--input $(SITE_ABS)/$(OPENAPI_YAML)

  

--output $(SITE_ABS)/$(SPEC_MD_DIR)

  

--nav $(SITE_ABS)/$(SCHEMA_NAV) @echo " > Markdown generation complete!"

  

gendocs: genmd @echo " > Copying schema pages to $(SCHEMA_DIR)/ for website ..." @mkdir -p $(SCHEMA_DIR) @sh "$(TOOLS_DIR)/scripts/parse-nav.sh" "$(SCHEMA_NAV)" list-pages | while IFS='|' read -r filename title; do

  

if [ -f "$(SPEC_MD_DIR)/$$filename.md" ]; then \ { \ echo "---"; \ echo "layout: page"; \ echo "title: $$title"; \ echo "---"; \ echo ""; \ cat "$(SPEC_MD_DIR)/$$filename.md";

  

} > "$(SCHEMA_DIR)/$$filename.md"; \ fi; \ done @echo " > Updating schema list in $(SCHEMA_DIR)/index.md ..." @if [ -f "$(SCHEMA_DIR)/index.md" ]; then \ schema_list_file="$(SCHEMA_DIR)/index.md.schema_list.tmp";

  

sh "$(TOOLS_DIR)/scripts/parse-nav.sh" "$(SCHEMA_NAV)" list-pages | while IFS='|' read -r filename title; do

  

[ -f "$(SCHEMA_DIR)/$$filename.md" ] && echo "- [$$title]($$filename.html)"; \ done > "$$schema_list_file"; \ awk -v list_file="$$schema_list_file" ' \ BEGIN { \ while ((getline line < list_file) > 0) { \ schema_list = schema_list line "\n"; \ } \ close(list_file); \ } \ /<!-- SCHEMA_LIST_START -->/ { \ print; \ print ""; \ printf "%s", schema_list; \ print ""; \ skip=1; \ next \ } \ /<!-- SCHEMA_LIST_END -->/ { print; skip=0; next } \ skip==0 { print } \ ' "$(SCHEMA_DIR)/index.md" > "$(SCHEMA_DIR)/index.md.tmp" && \ rm -f "$$schema_list_file" && \ mv "$(SCHEMA_DIR)/index.md.tmp" "$(SCHEMA_DIR)/index.md";

  

fi @echo " > Generating definitions table from lexicon ..." @if [ -f "model/02-definitions.md.template" ]; then

  

cp "model/02-definitions.md.template" "model/02-definitions.md";

  

fi @cd $(TOOLS_DIR) && go run . lexicon2md

  

--lexicon $(SITE_ABS)/lexicon.yaml

  

--output $(SITE_ABS)/model/02-definitions.md @echo " > Linking defined terms across documentation ..." @cd $(TOOLS_DIR) && go run . termlinker

  

--lexicon $(SITE_ABS)/lexicon.yaml

  

--docs $(SITE_ABS) @echo " > Documentation generation complete!" @$(MAKE) build-demo

  

build-demo: @echo " > Building @gemara/react live demo bundle ..." @npm --prefix $(TOOLS_DIR)/demo ci --ignore-scripts --no-audit --no-fund @npm --prefix $(TOOLS_DIR)/demo run build

  

serve: check-jekyll gendocs @echo " > Starting Jekyll documentation site..." @bundle exec jekyll serve --host 0.0.0.0 --livereload

  

build: check-jekyll gendocs @echo " > Building Jekyll documentation site..." @bundle exec jekyll build

  

test-links: @echo " > Validating all site pages and links with html-proofer..." @bundle exec htmlproofer _site

  

--allow-hash-href

  

--disable-external

  

--ignore-empty-alt

  

--only-4xx

  

--ignore-files '/model/02-definitions.html/'

  

--root-dir "$$(pwd)/_site"

  

cleanup-links: @echo " > Removing termlinker-generated links from documentation ..." @cd $(TOOLS_DIR) && go run . termlinker

  

--lexicon $(SITE_ABS)/lexicon.yaml

  

--docs $(SITE_ABS)

  

--cleanup @echo " > Link cleanup complete!"

  

cleanup: cleanup-links @echo " > Removing generated documentation files and links..." @sh "$(TOOLS_DIR)/scripts/parse-nav.sh" "$(SCHEMA_NAV)" list-pages | while IFS='|' read -r filename title; do

  

rm -f "$(SCHEMA_DIR)/$$filename.md";

  

done @rm -f model/02-definitions.md @git checkout -- $(SCHEMA_DIR)/index.md 2>/dev/null || true @rm -rf $(GENERATED_DIR) _site .jekyll-cache .jekyll-metadata assets/demo $(TOOLS_DIR)/demo/src/fixtures.generated.ts @echo " > Cleanup complete!" '@

  

# 11. Install dependencies, run build, and run TypeScript check

npm --prefix tools/demo install --ignore-scripts --no-audit --no-fund npm --prefix tools/demo run build npx --prefix tools/demo tsc -p tools/demo/tsconfig.json

  

# 12. Remove only assets/demo so git working tree stays clean while VS Code keeps node_modules & types

Remove-Item -Recurse -Force assets/demo -ErrorAction SilentlyContinue

  

# 13. Fix branch name typo (reat -> react) and check status

git branch -M feat/embed-gemara-react-live-demo git status

````

---

### Commit & Push (When Ready)

Once you see `> Built @gemara/react demo bundle (7 verified artifacts) -> assets/demo/` and `git status` shows your clean files on `feat/embed-gemara-react-live-demo`, run:

```powershell
git add .
git commit -s -m "feat: embed @gemara/react live demo in the website"
git push -u origin feat/embed-gemara-react-live-demo
````