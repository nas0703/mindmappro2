import { useState, useRef, useEffect, useReducer, useCallback } from "react";

// ════════════════════════════════════════════════════════════════════════════
// CONSTANTS & PALETTE
// ════════════════════════════════════════════════════════════════════════════
const PALETTE = [
  { id:"blue",   hex:"#4f8ef7" },
  { id:"violet", hex:"#8b5cf6" },
  { id:"rose",   hex:"#f43f5e" },
  { id:"amber",  hex:"#f59e0b" },
  { id:"teal",   hex:"#14b8a6" },
  { id:"cyan",   hex:"#06b6d4" },
  { id:"lime",   hex:"#84cc16" },
];
const palHex = (id) => (PALETTE.find(p => p.id === id) || PALETTE[0]).hex;

// Branch color auto-assigned by code category
const CATEGORY_COLORS = {
  imports:"cyan", constants:"amber", functions:"blue",
  classes:"violet", state:"teal", exports:"lime", other:"rose",
};

const NODE_W = 130;
const NODE_H = 38;
const GROUP_PAD = 28;
const LS_MAPS    = "mindmap-maps";
const LS_MAP_PRE = "mindmap-map-";
const LS_ACTIVE  = "mindmap-active";
const LS_THEME   = "mindmap-theme";

// ════════════════════════════════════════════════════════════════════════════
// UID — global, persists across map loads
// ════════════════════════════════════════════════════════════════════════════
let _uid = 100;
const nextId = () => ++_uid;
const makeMapId = () => `map_${Date.now()}_${Math.random().toString(36).slice(2,7)}`;

// ════════════════════════════════════════════════════════════════════════════
// INITIAL MAP
// ════════════════════════════════════════════════════════════════════════════
const makeInitNodes = () => [
  { id:1, label:"Central Idea", parentId:null, color:"blue",   notes:"", x:0,    y:0,    collapsed:false },
  { id:2, label:"Topic One",    parentId:1,    color:"violet", notes:"", x:-260, y:-120, collapsed:false },
  { id:3, label:"Topic Two",    parentId:1,    color:"rose",   notes:"", x:260,  y:-120, collapsed:false },
  { id:4, label:"Topic Three",  parentId:1,    color:"amber",  notes:"", x:-260, y:120,  collapsed:false },
  { id:5, label:"Topic Four",   parentId:1,    color:"teal",   notes:"", x:260,  y:120,  collapsed:false },
  { id:6, label:"Sub-idea A",   parentId:2,    color:"violet", notes:"", x:-470, y:-180, collapsed:false },
  { id:7, label:"Sub-idea B",   parentId:2,    color:"violet", notes:"", x:-470, y:-60,  collapsed:false },
  { id:8, label:"Detail",       parentId:3,    color:"rose",   notes:"", x:470,  y:-60,  collapsed:false },
  { id:9, label:"Detail",       parentId:4,    color:"amber",  notes:"", x:-470, y:180,  collapsed:false },
];

// ════════════════════════════════════════════════════════════════════════════
// LOCALSTORAGE HELPERS
// ════════════════════════════════════════════════════════════════════════════
const lsGet  = (k, fb) => { try { const v = localStorage.getItem(k); return v ? JSON.parse(v) : fb; } catch { return fb; } };
const lsSet  = (k, v)  => { try { localStorage.setItem(k, JSON.stringify(v)); } catch {} };

function loadAllMaps()   { return lsGet(LS_MAPS, []); }
function saveAllMaps(ms) { lsSet(LS_MAPS, ms); }
function loadMapData(id) { return lsGet(LS_MAP_PRE + id, null); }
function saveMapData(id, data) { lsSet(LS_MAP_PRE + id, data); }
function deleteMapData(id) { try { localStorage.removeItem(LS_MAP_PRE + id); } catch {} }

function getOrCreateActiveMap() {
  const maps = loadAllMaps();
  const activeId = lsGet(LS_ACTIVE, null);
  if (activeId && maps.find(m => m.id === activeId)) {
    const data = loadMapData(activeId);
    if (data) return { maps, activeId, nodes: data.nodes, groups: data.groups || [] };
  }
  // No valid active map — create fresh
  const freshMap = createFreshMapMeta("Untitled Map");
  const nodes = makeInitNodes();
  const groups = [];
  saveMapData(freshMap.id, { nodes, groups });
  const newMaps = [freshMap];
  saveAllMaps(newMaps);
  lsSet(LS_ACTIVE, freshMap.id);
  return { maps: newMaps, activeId: freshMap.id, nodes, groups };
}

function createFreshMapMeta(name) {
  return { id: makeMapId(), name, nodeCount: 1, createdAt: Date.now(), updatedAt: Date.now(), preview: "Central Idea" };
}

// ════════════════════════════════════════════════════════════════════════════
// TREE HELPERS
// ════════════════════════════════════════════════════════════════════════════
const getNode     = (ns, id) => ns.find(n => n.id === id);
const getChildren = (ns, id) => ns.filter(n => n.parentId === id);
const getRoot     = (ns)     => ns.find(n => n.parentId === null);

function getDepth(ns, id) {
  let d = 0, n = getNode(ns, id);
  while (n?.parentId) { d++; n = getNode(ns, n.parentId); }
  return d;
}
function allDescendants(ns, id) {
  const out = [], q = [id];
  while (q.length) {
    const cur = q.shift();
    getChildren(ns, cur).forEach(c => { out.push(c.id); q.push(c.id); });
  }
  return out;
}
function isVisible(ns, id) {
  const n = getNode(ns, id);
  if (!n) return false;
  if (!n.parentId) return true;
  const p = getNode(ns, n.parentId);
  if (!p || p.collapsed) return false;
  return isVisible(ns, p.id);
}
function branchColor(ns, id) {
  let n = getNode(ns, id);
  while (n) {
    if (n.color) return n.color;
    if (!n.parentId) return "blue";
    n = getNode(ns, n.parentId);
  }
  return "blue";
}
function treeMaxDepth(ns) {
  return ns.reduce((m, n) => Math.max(m, getDepth(ns, n.id)), 0);
}
function spawnPos(ns, parentId) {
  const p = getNode(ns, parentId);
  if (!p) return { x:200, y:0 };
  const kids = getChildren(ns, parentId);
  const dir  = p.x >= 0 ? 1 : -1;
  return { x: p.x + dir * 210, y: p.y + kids.length * 60 };
}

// ════════════════════════════════════════════════════════════════════════════
// GROUP HELPERS — bounds always derived, never stored
// ════════════════════════════════════════════════════════════════════════════
function groupBounds(nodeIds, ns) {
  const members = nodeIds.map(id => getNode(ns, id)).filter(Boolean);
  if (!members.length) return null;
  const minX = Math.min(...members.map(n => n.x)) - NODE_W / 2 - GROUP_PAD;
  const minY = Math.min(...members.map(n => n.y)) - NODE_H / 2 - GROUP_PAD;
  const maxX = Math.max(...members.map(n => n.x)) + NODE_W / 2 + GROUP_PAD;
  const maxY = Math.max(...members.map(n => n.y)) + NODE_H / 2 + GROUP_PAD;
  return { x: minX, y: minY, w: maxX - minX, h: maxY - minY };
}

function nodeInsideBounds(node, bounds) {
  return node.x >= bounds.x && node.x <= bounds.x + bounds.w &&
         node.y >= bounds.y && node.y <= bounds.y + bounds.h;
}

// After any MOVE, recompute which nodes still belong to each group
function recomputeGroups(groups, nodes) {
  return groups.map(g => {
    const bounds = groupBounds(g.nodeIds, nodes);
    if (!bounds) return g;
    const remaining = g.nodeIds.filter(id => {
      const n = getNode(nodes, id);
      return n && nodeInsideBounds(n, bounds);
    });
    return { ...g, nodeIds: remaining };
  }).filter(g => g.nodeIds.length >= 2);
}

// Auto-layout for code-imported trees (radial)
function radialLayout(tree, cx = 0, cy = 0, depth = 0, angleStart = 0, angleEnd = 2 * Math.PI) {
  const nodes = [];
  const id = nextId();
  nodes.push({ id, label: tree.label, parentId: tree._parentId || null, color: tree.color || "blue", notes: tree.note || "", x: cx, y: cy, collapsed: false });
  if (tree.children?.length) {
    const r = 200 + depth * 60;
    const slice = (angleEnd - angleStart) / tree.children.length;
    tree.children.forEach((child, i) => {
      child._parentId = id;
      const angle = angleStart + slice * i + slice / 2;
      const childX = cx + Math.cos(angle) * r;
      const childY = cy + Math.sin(angle) * r;
      const sub = radialLayout(child, childX, childY, depth + 1, angle - slice / 2, angle + slice / 2);
      nodes.push(...sub);
    });
  }
  return nodes;
}

// ════════════════════════════════════════════════════════════════════════════
// REDUCER
// ════════════════════════════════════════════════════════════════════════════
function reducer(state, action) {
  const { nodes, groups, history, future } = state;
  const snap = () => [...history.slice(-49), JSON.stringify({ nodes, groups })];

  switch (action.type) {

    case "ADD_CHILD": {
      const pos  = spawnPos(nodes, action.parentId);
      const p    = getNode(nodes, action.parentId);
      const node = { id:nextId(), label:"New Node", parentId:action.parentId, color:p?.color||"blue", notes:"", ...pos, collapsed:false };
      return { ...state, nodes:[...nodes, node], selectedId:node.id, editingId:node.id, history:snap(), future:[] };
    }
    case "ADD_SIBLING": {
      const n = getNode(nodes, action.id);
      if (!n?.parentId) return state;
      const pos  = spawnPos(nodes, n.parentId);
      const node = { id:nextId(), label:"New Node", parentId:n.parentId, color:n.color, notes:"", ...pos, collapsed:false };
      return { ...state, nodes:[...nodes, node], selectedId:node.id, editingId:node.id, history:snap(), future:[] };
    }
    case "DELETE": {
      const n = getNode(nodes, action.id);
      if (!n?.parentId) return state;
      const kill     = new Set([action.id, ...allDescendants(nodes, action.id)]);
      const newNodes = nodes.filter(n => !kill.has(n.id));
      const newGroups = groups.map(g => ({ ...g, nodeIds: g.nodeIds.filter(id => !kill.has(id)) })).filter(g => g.nodeIds.length >= 2);
      return { ...state, nodes:newNodes, groups:newGroups, selectedId:getRoot(nodes)?.id ?? null, history:snap(), future:[] };
    }
    case "RENAME":
      return { ...state, nodes:nodes.map(n => n.id===action.id ? {...n, label:action.label||n.label} : n), editingId:null, history:snap(), future:[] };
    case "SET_NOTES":
      return { ...state, nodes:nodes.map(n => n.id===action.id ? {...n, notes:action.notes} : n) };
    case "SET_COLOR":
      return { ...state, nodes:nodes.map(n => n.id===action.id ? {...n, color:action.color} : n), history:snap(), future:[] };
    case "MOVE": {
      const newNodes  = nodes.map(n => n.id===action.id ? {...n, x:action.x, y:action.y} : n);
      const newGroups = recomputeGroups(groups, newNodes);
      return { ...state, nodes:newNodes, groups:newGroups };
    }
    case "TOGGLE_COLLAPSE":
      return { ...state, nodes:nodes.map(n => n.id===action.id ? {...n, collapsed:!n.collapsed} : n), history:snap(), future:[] };

    // ── Groups ──
    case "ADD_GROUP": {
      let gCount = state.groupCount + 1;
      const group = { id:nextId(), label:`Group ${gCount}`, nodeIds:action.nodeIds, collapsed:false };
      return { ...state, groups:[...groups, group], selectedGroupId:group.id, editingGroupId:group.id, groupCount:gCount, history:snap(), future:[] };
    }
    case "RENAME_GROUP":
      return { ...state, groups:groups.map(g => g.id===action.id ? {...g, label:action.label||g.label} : g), editingGroupId:null, history:snap(), future:[] };
    case "DELETE_GROUP":
      return { ...state, groups:groups.filter(g => g.id!==action.id), selectedGroupId:null, history:snap(), future:[] };
    case "TOGGLE_GROUP_COLLAPSE":
      return { ...state, groups:groups.map(g => g.id===action.id ? {...g, collapsed:!g.collapsed} : g), history:snap(), future:[] };
    case "SELECT_GROUP":
      return { ...state, selectedGroupId:action.id, selectedId:null };

    case "SELECT":      return { ...state, selectedId:action.id, selectedGroupId:null };
    case "START_EDIT":  return { ...state, editingId:action.id };
    case "STOP_EDIT":   return { ...state, editingId:null };
    case "START_EDIT_GROUP": return { ...state, editingGroupId:action.id };
    case "STOP_EDIT_GROUP":  return { ...state, editingGroupId:null };

    case "UNDO": {
      if (!history.length) return state;
      const prev = JSON.parse(history[history.length-1]);
      return { ...state, ...prev, history:history.slice(0,-1), future:[JSON.stringify({nodes,groups}), ...future], editingId:null };
    }
    case "REDO": {
      if (!future.length) return state;
      const next = JSON.parse(future[0]);
      return { ...state, ...next, future:future.slice(1), history:[...history, JSON.stringify({nodes,groups})], editingId:null };
    }

    // ── Map-level ──
    case "LOAD_MAP":
      return { ...state, nodes:action.nodes, groups:action.groups||[], selectedId:getRoot(action.nodes)?.id??null, selectedGroupId:null, editingId:null, editingGroupId:null, history:[], future:[] };

    default: return state;
  }
}

// ════════════════════════════════════════════════════════════════════════════
// INIT STATE — bootstrapped from localStorage
// ════════════════════════════════════════════════════════════════════════════
function buildInitState() {
  const { maps, activeId, nodes, groups } = getOrCreateActiveMap();
  return {
    nodes, groups,
    selectedId: getRoot(nodes)?.id ?? null,
    selectedGroupId: null,
    editingId: null,
    editingGroupId: null,
    history: [], future: [],
    groupCount: 0,
  };
}

// ════════════════════════════════════════════════════════════════════════════
// THEME TOKENS
// ════════════════════════════════════════════════════════════════════════════
const darkTheme = {
  bg:"#0c0e14", surface:"#131720", surface2:"#1a1f2e", surface3:"#222840",
  border:"rgba(255,255,255,0.07)", border2:"rgba(255,255,255,0.13)",
  text:"#e4e8f4", text2:"#7a82a0", text3:"#454d68",
  nodeText:"#e8eaf4", nodeBg:(c)=>`${palHex(c)}28`,
  groupFill:(c)=>`${palHex(c)}0a`, groupStroke:(c)=>`${palHex(c)}40`,
};
const lightTheme = {
  bg:"#eef1f8", surface:"#ffffff", surface2:"#f4f6ff", surface3:"#e6eaf8",
  border:"rgba(0,0,0,0.07)", border2:"rgba(0,0,0,0.13)",
  text:"#1a1e30", text2:"#5a6180", text3:"#9098be",
  nodeText:"#1a1e30", nodeBg:(c)=>`${palHex(c)}18`,
  groupFill:(c)=>`${palHex(c)}08`, groupStroke:(c)=>`${palHex(c)}35`,
};

// ════════════════════════════════════════════════════════════════════════════
// RELATIVE TIME
// ════════════════════════════════════════════════════════════════════════════
function relTime(ts) {
  const s = Math.floor((Date.now() - ts) / 1000);
  if (s < 60)   return "just now";
  if (s < 3600) return `${Math.floor(s/60)}m ago`;
  if (s < 86400)return `${Math.floor(s/3600)}h ago`;
  return `${Math.floor(s/86400)}d ago`;
}

// ════════════════════════════════════════════════════════════════════════════
// APP
// ════════════════════════════════════════════════════════════════════════════
export default function MindMapPro() {
  const [state, dispatch] = useReducer(reducer, null, buildInitState);
  const { nodes, groups, selectedId, selectedGroupId, editingId, editingGroupId } = state;

  // ── Persistent theme ──────────────────────────────────────────────────────
  const [dark, setDark] = useState(() => {
    const s = localStorage.getItem(LS_THEME);
    return s ? s === "dark" : true;
  });
  const toggleTheme = () => {
    setDark(d => {
      const next = !d;
      localStorage.setItem(LS_THEME, next ? "dark" : "light");
      return next;
    });
  };
  const th = dark ? darkTheme : lightTheme;
  const accent = "#4f8ef7";

  // ── Map library state ─────────────────────────────────────────────────────
  const [maps,      setMaps]      = useState(() => loadAllMaps());
  const [activeMapId, setActiveMapId] = useState(() => lsGet(LS_ACTIVE, null));
  const [drawerOpen, setDrawerOpen] = useState(false);
  const [mapSearch,  setMapSearch]  = useState("");
  const [cardMenu,   setCardMenu]   = useState(null); // { id, x, y }
  const [renamingMapId, setRenamingMapId] = useState(null);
  const [renameVal,  setRenameVal]  = useState("");

  // ── Save status ───────────────────────────────────────────────────────────
  const [saveStatus, setSaveStatus] = useState("saved"); // "pending" | "saving" | "saved"
  const saveTimer = useRef(null);

  // ── Canvas state ──────────────────────────────────────────────────────────
  const [pan,  setPan]  = useState({ x:0, y:0 });
  const [zoom, setZoom] = useState(0.9);

  // ── UI state ──────────────────────────────────────────────────────────────
  const [panelOpen,  setPanelOpen]  = useState(true);
  const [ctxMenu,    setCtxMenu]    = useState(null);
  const [toast,      setToast]      = useState("");
  const [editVal,    setEditVal]    = useState("");
  const [groupMode,  setGroupMode]  = useState(false);   // lasso draw mode
  const [lasso,      setLasso]      = useState(null);    // { sx,sy,ex,ey } screen coords
  const [codeModal,  setCodeModal]  = useState(false);
  const [codeText,   setCodeText]   = useState("");
  const [codeLoading,setCodeLoading]= useState(false);
  const [detectedLang,setDetectedLang]=useState("—");

  // ── Refs ──────────────────────────────────────────────────────────────────
  const svgRef      = useRef(null);
  const editRef     = useRef(null);
  const editGrpRef  = useRef(null);
  const toastTimer  = useRef(null);
  const panRef      = useRef(pan);
  const zoomRef     = useRef(zoom);
  const nodesRef    = useRef(nodes);
  const editingRef  = useRef(editingId);
  const editGrpRef2 = useRef(editingGroupId);
  const panActive   = useRef(null);
  const lassoStart  = useRef(null);
  const groupModeRef= useRef(groupMode);

  panRef.current      = pan;
  zoomRef.current     = zoom;
  nodesRef.current    = nodes;
  editingRef.current  = editingId;
  editGrpRef2.current = editingGroupId;
  groupModeRef.current= groupMode;

  // ════════════════════════════════════════════════════════════════════════
  // AUTO-SAVE — debounced 2s after any node/group change
  // ════════════════════════════════════════════════════════════════════════
  const flushSave = useCallback((id, ns, gs, mapsArr) => {
    saveMapData(id, { nodes: ns, groups: gs });
    const now = Date.now();
    const updated = mapsArr.map(m => m.id === id
      ? { ...m, nodeCount: ns.length, updatedAt: now, preview: getRoot(ns)?.label || "Map" }
      : m
    ).sort((a, b) => b.updatedAt - a.updatedAt);
    saveAllMaps(updated);
    setMaps(updated);
    setSaveStatus("saved");
  }, []);

  useEffect(() => {
    if (!activeMapId) return;
    setSaveStatus("pending");
    clearTimeout(saveTimer.current);
    saveTimer.current = setTimeout(() => {
      setSaveStatus("saving");
      flushSave(activeMapId, nodes, groups, maps);
    }, 2000);
    return () => clearTimeout(saveTimer.current);
  }, [nodes, groups]);

  // ════════════════════════════════════════════════════════════════════════
  // MAP OPERATIONS
  // ════════════════════════════════════════════════════════════════════════
  const switchMap = useCallback((id) => {
    // Flush current map immediately
    clearTimeout(saveTimer.current);
    flushSave(activeMapId, nodesRef.current, state.groups, maps);

    const data = loadMapData(id);
    if (!data) return;
    dispatch({ type:"LOAD_MAP", nodes:data.nodes, groups:data.groups||[] });
    setActiveMapId(id);
    lsSet(LS_ACTIVE, id);
    setDrawerOpen(false);
    setSaveStatus("saved");
    // Fit view after load
    setTimeout(() => fitViewFor(data.nodes), 80);
  }, [activeMapId, maps, state.groups]);

  const createNewMap = useCallback(() => {
    clearTimeout(saveTimer.current);
    flushSave(activeMapId, nodesRef.current, state.groups, maps);

    const meta   = createFreshMapMeta("Untitled Map");
    const ns     = makeInitNodes();
    saveMapData(meta.id, { nodes: ns, groups: [] });
    const updated = [meta, ...maps].sort((a,b) => b.updatedAt - a.updatedAt);
    saveAllMaps(updated);
    setMaps(updated);
    dispatch({ type:"LOAD_MAP", nodes:ns, groups:[] });
    setActiveMapId(meta.id);
    lsSet(LS_ACTIVE, meta.id);
    setDrawerOpen(false);
    setSaveStatus("saved");
    setTimeout(() => fitViewFor(ns), 80);
  }, [activeMapId, maps, state.groups]);

  const duplicateMap = useCallback((id) => {
    const src  = maps.find(m => m.id === id);
    const data = loadMapData(id);
    if (!src || !data) return;
    const meta  = { ...createFreshMapMeta(`Copy of ${src.name}`), nodeCount:src.nodeCount };
    saveMapData(meta.id, { nodes:data.nodes, groups:data.groups||[] });
    const updated = [...maps, meta].sort((a,b) => b.updatedAt - a.updatedAt);
    saveAllMaps(updated);
    setMaps(updated);
    showToast("Map duplicated ✓");
  }, [maps]);

  const deleteMap = useCallback((id) => {
    deleteMapData(id);
    const remaining = maps.filter(m => m.id !== id);
    if (!remaining.length) {
      // Always keep at least one map
      const meta = createFreshMapMeta("Untitled Map");
      const ns   = makeInitNodes();
      saveMapData(meta.id, { nodes:ns, groups:[] });
      remaining.push(meta);
    }
    saveAllMaps(remaining);
    setMaps(remaining);
    if (id === activeMapId) {
      const next = remaining[0];
      const data = loadMapData(next.id);
      dispatch({ type:"LOAD_MAP", nodes:data.nodes, groups:data.groups||[] });
      setActiveMapId(next.id);
      lsSet(LS_ACTIVE, next.id);
      setSaveStatus("saved");
    }
    showToast("Map deleted");
  }, [maps, activeMapId]);

  const commitMapRename = useCallback(() => {
    if (!renamingMapId || !renameVal.trim()) { setRenamingMapId(null); return; }
    const updated = maps.map(m => m.id === renamingMapId ? { ...m, name: renameVal.trim() } : m);
    saveAllMaps(updated);
    setMaps(updated);
    setRenamingMapId(null);
  }, [maps, renamingMapId, renameVal]);

  // ════════════════════════════════════════════════════════════════════════
  // TOAST
  // ════════════════════════════════════════════════════════════════════════
  const showToast = useCallback((msg) => {
    setToast(msg);
    clearTimeout(toastTimer.current);
    toastTimer.current = setTimeout(() => setToast(""), 2400);
  }, []);

  // ════════════════════════════════════════════════════════════════════════
  // INLINE EDIT — nodes
  // ════════════════════════════════════════════════════════════════════════
  const startEdit = useCallback((id) => {
    const n = getNode(nodesRef.current, id);
    if (!n) return;
    setEditVal(n.label);
    dispatch({ type:"START_EDIT", id });
  }, []);

  const commitEdit = useCallback(() => {
    const id  = editingRef.current;
    const val = editRef.current?.value?.trim();
    if (id && val) dispatch({ type:"RENAME", id, label:val });
    else           dispatch({ type:"STOP_EDIT" });
  }, []);

  useEffect(() => { if (editingId) { editRef.current?.focus(); editRef.current?.select(); } }, [editingId]);

  // ════════════════════════════════════════════════════════════════════════
  // INLINE EDIT — groups
  // ════════════════════════════════════════════════════════════════════════
  const startEditGroup = useCallback((id) => {
    const g = groups.find(g => g.id === id);
    if (!g) return;
    setEditVal(g.label);
    dispatch({ type:"START_EDIT_GROUP", id });
  }, [groups]);

  const commitEditGroup = useCallback(() => {
    const id  = editGrpRef2.current;
    const val = editGrpRef.current?.value?.trim();
    if (id && val) dispatch({ type:"RENAME_GROUP", id, label:val });
    else           dispatch({ type:"STOP_EDIT_GROUP" });
  }, []);

  useEffect(() => { if (editingGroupId) { editGrpRef.current?.focus(); editGrpRef.current?.select(); } }, [editingGroupId]);

  // ════════════════════════════════════════════════════════════════════════
  // PAN — global mouse listeners via ref
  // ════════════════════════════════════════════════════════════════════════
  useEffect(() => {
    const onMove = (e) => {
      if (panActive.current) {
        const { mx, my, px, py } = panActive.current;
        setPan({ x: px + (e.clientX - mx), y: py + (e.clientY - my) });
      }
      // Live lasso update
      if (lassoStart.current) {
        const rect = svgRef.current?.getBoundingClientRect();
        if (!rect) return;
        setLasso({ ...lassoStart.current, ex: e.clientX - rect.left, ey: e.clientY - rect.top });
      }
    };
    const onUp = (e) => {
      panActive.current = null;
      // Commit lasso
      if (lassoStart.current && svgRef.current) {
        const rect = svgRef.current.getBoundingClientRect();
        const ex = e.clientX - rect.left, ey = e.clientY - rect.top;
        const { sx, sy } = lassoStart.current;
        const lx = Math.min(sx, ex), ly = Math.min(sy, ey);
        const lw = Math.abs(ex - sx),  lh = Math.abs(ey - sy);
        if (lw > 10 && lh > 10) {
          // Hit-test nodes in lasso (screen coords)
          const captured = nodesRef.current.filter(n => {
            if (!isVisible(nodesRef.current, n.id)) return false;
            const sc = worldToScreen(n.x, n.y, rect);
            return sc.x >= lx && sc.x <= lx + lw && sc.y >= ly && sc.y <= ly + lh;
          });
          if (captured.length >= 2) {
            dispatch({ type:"ADD_GROUP", nodeIds: captured.map(n => n.id) });
          } else {
            showToast("Select at least 2 nodes to group");
          }
        }
        lassoStart.current = null;
        setLasso(null);
        setGroupMode(false);
      }
    };
    window.addEventListener("mousemove", onMove);
    window.addEventListener("mouseup",   onUp);
    return () => { window.removeEventListener("mousemove", onMove); window.removeEventListener("mouseup", onUp); };
  }, [showToast]);

  // ════════════════════════════════════════════════════════════════════════
  // WHEEL ZOOM
  // ════════════════════════════════════════════════════════════════════════
  useEffect(() => {
    const el = svgRef.current; if (!el) return;
    const onWheel = (e) => {
      e.preventDefault();
      const rect   = el.getBoundingClientRect();
      const mx     = e.clientX - rect.left  - rect.width  / 2;
      const my     = e.clientY - rect.top   - rect.height / 2;
      const factor = e.deltaY < 0 ? 1.1 : 0.91;
      setZoom(z => {
        const nz = Math.max(0.12, Math.min(3, z * factor));
        setPan(p => ({ x: mx + (p.x - mx) * (nz/z), y: my + (p.y - my) * (nz/z) }));
        return nz;
      });
    };
    el.addEventListener("wheel", onWheel, { passive:false });
    return () => el.removeEventListener("wheel", onWheel);
  }, []);

  // ════════════════════════════════════════════════════════════════════════
  // WORLD ↔ SCREEN
  // ════════════════════════════════════════════════════════════════════════
  function worldToScreen(wx, wy, rect) {
    const r = rect || svgRef.current?.getBoundingClientRect() || { width:800, height:600 };
    return { x: wx * zoomRef.current + r.width / 2 + panRef.current.x, y: wy * zoomRef.current + r.height / 2 + panRef.current.y };
  }

  // ════════════════════════════════════════════════════════════════════════
  // NODE DRAG
  // ════════════════════════════════════════════════════════════════════════
  const makeNodeDown = useCallback((id) => (e) => {
    if (e.button !== 0) return;
    e.stopPropagation();
    dispatch({ type:"SELECT", id });
    panActive.current = null;

    const n = getNode(nodesRef.current, id);
    if (!n) return;
    const startMx = e.clientX, startMy = e.clientY;
    const origX = n.x, origY = n.y;
    let moved = false;

    const onMove = (me) => {
      const dx = (me.clientX - startMx) / zoomRef.current;
      const dy = (me.clientY - startMy) / zoomRef.current;
      if (!moved && Math.abs(dx) < 3 && Math.abs(dy) < 3) return;
      moved = true;
      dispatch({ type:"MOVE", id, x: origX + dx, y: origY + dy });
    };
    const onUp = () => {
      window.removeEventListener("mousemove", onMove);
      window.removeEventListener("mouseup",   onUp);
    };
    window.addEventListener("mousemove", onMove);
    window.addEventListener("mouseup",   onUp);
  }, []);

  // ════════════════════════════════════════════════════════════════════════
  // SVG BACKGROUND MOUSEDOWN — pan or lasso
  // ════════════════════════════════════════════════════════════════════════
  const onBgDown = (e) => {
    if (e.button !== 0) return;
    if (groupModeRef.current) {
      // Start lasso
      const rect = svgRef.current?.getBoundingClientRect();
      if (!rect) return;
      lassoStart.current = { sx: e.clientX - rect.left, sy: e.clientY - rect.top, ex: e.clientX - rect.left, ey: e.clientY - rect.top };
      setLasso({ sx: e.clientX - rect.left, sy: e.clientY - rect.top, ex: e.clientX - rect.left, ey: e.clientY - rect.top });
    } else {
      panActive.current = { mx:e.clientX, my:e.clientY, px:panRef.current.x, py:panRef.current.y };
    }
  };

  // ════════════════════════════════════════════════════════════════════════
  // FIT VIEW
  // ════════════════════════════════════════════════════════════════════════
  function fitViewFor(ns) {
    const vis = ns.filter(n => isVisible(ns, n.id));
    if (!vis.length) return;
    const el = svgRef.current; if (!el) return;
    const { width, height } = el.getBoundingClientRect();
    const xs = vis.map(n => n.x), ys = vis.map(n => n.y);
    const minX = Math.min(...xs), maxX = Math.max(...xs);
    const minY = Math.min(...ys), maxY = Math.max(...ys);
    const pw = maxX - minX + 260, ph = maxY - minY + 160;
    const z  = Math.min(1.3, Math.min(width/pw, height/ph) * 0.88);
    setZoom(z);
    setPan({ x: -(minX+maxX)/2*z, y: -(minY+maxY)/2*z });
  }
  const fitView = useCallback(() => fitViewFor(nodesRef.current), []);

  // ════════════════════════════════════════════════════════════════════════
  // KEYBOARD
  // ════════════════════════════════════════════════════════════════════════
  useEffect(() => {
    const onKey = (e) => {
      const tag = document.activeElement?.tagName;
      if (tag === "INPUT" || tag === "TEXTAREA") return;
      if (e.key === "Escape")  { setGroupMode(false); lassoStart.current=null; setLasso(null); setCtxMenu(null); setDrawerOpen(false); }
      if (e.key === "g" || e.key === "G") { e.preventDefault(); setGroupMode(m => !m); }
      if (e.key === "Tab")    { e.preventDefault(); dispatch({type:"ADD_CHILD",   parentId:selectedId}); }
      if (e.key === "Enter")  { e.preventDefault(); dispatch({type:"ADD_SIBLING", id:selectedId}); }
      if (e.key === "Delete" || e.key === "Backspace") {
        e.preventDefault();
        if (selectedGroupId) dispatch({type:"DELETE_GROUP", id:selectedGroupId});
        else                 dispatch({type:"DELETE",       id:selectedId});
      }
      if (e.key === "F2") {
        if (selectedGroupId) startEditGroup(selectedGroupId);
        else                 startEdit(selectedId);
      }
      if (e.ctrlKey && e.key === "z") { e.preventDefault(); dispatch({type:"UNDO"}); }
      if (e.ctrlKey && (e.key === "y"||e.key==="Y")) { e.preventDefault(); dispatch({type:"REDO"}); }
      if (e.ctrlKey && e.shiftKey && e.key==="F") { e.preventDefault(); fitView(); }
    };
    window.addEventListener("keydown", onKey);
    return () => window.removeEventListener("keydown", onKey);
  }, [selectedId, selectedGroupId, startEdit, startEditGroup, fitView]);

  // ════════════════════════════════════════════════════════════════════════
  // EXPORT
  // ════════════════════════════════════════════════════════════════════════
  const exportSVG = () => {
    const el = svgRef.current; if (!el) return;
    const blob = new Blob([el.outerHTML], {type:"image/svg+xml"});
    Object.assign(document.createElement("a"), {href:URL.createObjectURL(blob), download:"mindmap.svg"}).click();
    showToast("SVG exported ✓");
  };
  const exportJSON = () => {
    const blob = new Blob([JSON.stringify({nodes,groups},null,2)], {type:"application/json"});
    Object.assign(document.createElement("a"), {href:URL.createObjectURL(blob), download:"mindmap.json"}).click();
    showToast("JSON exported ✓");
  };

  // ════════════════════════════════════════════════════════════════════════
  // CODE IMPORT
  // ════════════════════════════════════════════════════════════════════════
  const handleCodeImport = async () => {
    if (!codeText.trim()) return;
    setCodeLoading(true);
    setDetectedLang("—");
    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method:"POST",
        headers:{ "Content-Type":"application/json" },
        body: JSON.stringify({
          model:"claude-sonnet-4-20250514",
          max_tokens:4000,
          system:`You are a code analysis engine. Analyze code and return ONLY valid JSON — no markdown, no explanation, nothing else.`,
          messages:[{
            role:"user",
            content:`Analyze this code carefully. Return ONLY a JSON object (no markdown fences, no explanation) with this exact structure:
{
  "filename": "detected filename or 'Code'",
  "language": "detected language and framework e.g. JavaScript (React)",
  "root": "filename or main module name",
  "children": [
    {
      "label": "Imports",
      "category": "imports",
      "note": "brief description",
      "children": [
        { "label": "import name", "note": "what it is used for", "children": [] }
      ]
    },
    {
      "label": "Constants",
      "category": "constants",
      "note": "brief description",
      "children": [
        { "label": "CONST_NAME", "note": "what it stores", "children": [] }
      ]
    },
    {
      "label": "Functions",
      "category": "functions",
      "note": "brief description",
      "children": [
        {
          "label": "functionName()",
          "note": "what it does",
          "children": [
            { "label": "param: name", "note": "type and purpose", "children": [] }
          ]
        }
      ]
    },
    {
      "label": "Classes / Components",
      "category": "classes",
      "note": "brief description",
      "children": []
    },
    {
      "label": "State & Refs",
      "category": "state",
      "note": "brief description",
      "children": []
    },
    {
      "label": "Exports",
      "category": "exports",
      "note": "what is exported",
      "children": []
    }
  ]
}
Only include sections that exist in the code. Keep labels short (under 30 chars).

Code:
${codeText.slice(0, 12000)}`
          }]
        })
      });
      const data = await res.json();
      const raw  = data.content?.[0]?.text || "";
      // Strip any accidental markdown fences
      const clean = raw.replace(/```json|```/g,"").trim();
      const parsed = JSON.parse(clean);

      setDetectedLang(parsed.language || "Unknown");

      // Build nodes via radial layout
      const assignColors = (node) => {
        if (node.category) node.color = CATEGORY_COLORS[node.category] || "rose";
        node.children?.forEach(c => { c.color = c.color || node.color; assignColors(c); });
      };
      assignColors(parsed);

      const newNodes = radialLayout({ label: parsed.root || parsed.filename || "Code", color:"blue", children: parsed.children || [] });

      // Create new map
      const meta = createFreshMapMeta(parsed.filename || parsed.root || "Imported Code");
      saveMapData(meta.id, { nodes: newNodes, groups: [] });
      const updated = [meta, ...maps].sort((a,b) => b.updatedAt - a.updatedAt);
      saveAllMaps(updated);
      setMaps(updated);

      // Switch to it
      dispatch({ type:"LOAD_MAP", nodes:newNodes, groups:[] });
      setActiveMapId(meta.id);
      lsSet(LS_ACTIVE, meta.id);
      setSaveStatus("saved");

      setTimeout(() => fitViewFor(newNodes), 80);
      showToast(`Map generated — ${newNodes.length} nodes created ✓`);
      setCodeModal(false);
      setCodeText("");
      setDetectedLang("—");
    } catch (err) {
      showToast("Analysis failed — please try again");
    } finally {
      setCodeLoading(false);
    }
  };

  // ════════════════════════════════════════════════════════════════════════
  // DERIVED
  // ════════════════════════════════════════════════════════════════════════
  const selNode   = getNode(nodes, selectedId);
  const selGroup  = groups.find(g => g.id === selectedGroupId);
  const visNodes  = nodes.filter(n => isVisible(nodes, n.id));
  const visLinks  = nodes.filter(n => n.parentId && isVisible(nodes, n.id) && isVisible(nodes, n.parentId));
  const activeMap = maps.find(m => m.id === activeMapId);
  const filteredMaps = maps.filter(m => m.name.toLowerCase().includes(mapSearch.toLowerCase()));

  // Inline edit overlay position
  const editNode = editingId ? getNode(nodes, editingId) : null;
  let editOverlay = null;
  if (editNode && svgRef.current) {
    const { width, height } = svgRef.current.getBoundingClientRect();
    const sx = editNode.x * zoom + width  / 2 + pan.x;
    const sy = editNode.y * zoom + height / 2 + pan.y;
    const w  = Math.max(130, editVal.length * 9 + 34);
    editOverlay = { left: sx - w/2, top: sy - 20, width: w };
  }

  // ════════════════════════════════════════════════════════════════════════
  // RENDER
  // ════════════════════════════════════════════════════════════════════════
  return (
    <div style={{ display:"flex", flexDirection:"column", height:"100vh", width:"100%", background:th.bg, color:th.text, fontFamily:"'DM Sans','Segoe UI',sans-serif", overflow:"hidden" }}>

      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:wght@300;400;500;600&family=DM+Mono:wght@400;500&display=swap');
        *{box-sizing:border-box}
        ::-webkit-scrollbar{width:4px}
        ::-webkit-scrollbar-thumb{background:${th.border2};border-radius:4px}
        .tb:hover{background:${th.surface2}!important;color:${th.text}!important}
        .tb{transition:background .13s,color .13s}
        @keyframes selDash{to{stroke-dashoffset:-60}}
        @keyframes fadeUp{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:translateY(0)}}
        @keyframes slideIn{from{transform:translateX(-100%)}to{transform:translateX(0)}}
        @keyframes pulse{0%,100%{opacity:1}50%{opacity:.4}}
        .ctx-item:hover{background:${th.surface3}!important;color:${th.text}!important}
        .ctx-danger:hover{background:rgba(244,63,94,.13)!important;color:#f43f5e!important}
        .swatch:hover{transform:scale(1.22)!important}
        .swatch{transition:transform .14s,box-shadow .14s;cursor:pointer}
        .cdot{transition:r .14s}
        .cdot:hover{r:7.5}
        .map-card:hover{background:${th.surface2}!important}
        .map-card{transition:background .13s}
        .card-menu-btn{opacity:0;transition:opacity .13s}
        .map-card:hover .card-menu-btn{opacity:1}
        .panel-inp{transition:border-color .15s,box-shadow .15s;outline:none}
        .panel-inp:focus{border-color:${accent}!important;box-shadow:0 0 0 3px ${accent}22!important}
        .group-rect{transition:opacity .2s}
      `}</style>

      {/* ══ TOOLBAR ══════════════════════════════════════════════════════════ */}
      <div style={{ height:52, background:th.surface, borderBottom:`1px solid ${th.border}`, display:"flex", alignItems:"center", gap:2, padding:"0 12px", flexShrink:0, zIndex:50 }}>

        {/* Maps drawer button */}
        <Btn th={th} accent={accent} onClick={() => setDrawerOpen(true)} title="Maps library">
          <MapsIco/> <span style={{fontSize:12}}>Maps</span>
        </Btn>

        <Sep th={th}/>

        {/* Brand + active map name */}
        <span style={{ fontFamily:"'DM Serif Display',serif", fontStyle:"italic", fontSize:17, color:accent, letterSpacing:"-0.3px", whiteSpace:"nowrap" }}>✦</span>
        <button onClick={() => setDrawerOpen(true)} style={{ background:"none", border:"none", cursor:"pointer", display:"flex", alignItems:"center", gap:6, padding:"4px 8px", borderRadius:7 }}
          onMouseEnter={e=>e.currentTarget.style.background=th.surface2}
          onMouseLeave={e=>e.currentTarget.style.background="none"}>
          <span style={{ fontFamily:"'DM Serif Display',serif", fontStyle:"italic", fontSize:15, color:accent }}>MindMap Pro</span>
          {activeMap && <span style={{ fontSize:12, color:th.text3, maxWidth:140, overflow:"hidden", textOverflow:"ellipsis", whiteSpace:"nowrap" }}>— {activeMap.name}</span>}
        </button>

        <Sep th={th}/>

        <Btn primary th={th} accent={accent} title="Tab"   onClick={()=>dispatch({type:"ADD_CHILD",   parentId:selectedId})}><PlusIco/> Child</Btn>
        <Btn         th={th} accent={accent} title="Enter" onClick={()=>dispatch({type:"ADD_SIBLING", id:selectedId})}><MinusIco/> Sibling</Btn>
        <Btn         th={th} accent={accent} title="Del"   onClick={()=>dispatch({type:"DELETE",      id:selectedId})}><TrashIco/> Delete</Btn>

        <Sep th={th}/>
        <Btn th={th} accent={accent} title="Ctrl+Z" onClick={()=>dispatch({type:"UNDO"})}><UndoIco/> Undo</Btn>
        <Btn th={th} accent={accent} title="Ctrl+Y" onClick={()=>dispatch({type:"REDO"})}><RedoIco/> Redo</Btn>

        <Sep th={th}/>

        {/* Group mode toggle */}
        <Btn th={th} accent={accent} title="G — draw group box"
          style={{ color:groupMode?accent:th.text2, background:groupMode?`${accent}18`:"transparent", border:groupMode?`1px solid ${accent}44`:"1px solid transparent" }}
          onClick={()=>setGroupMode(m=>!m)}>
          <GroupIco/> Group
        </Btn>

        <Sep th={th}/>
        <Btn th={th} accent={accent} title="Ctrl+Shift+F" onClick={fitView}><FitIco/> Fit</Btn>
        <Btn th={th} accent={accent} onClick={()=>setZoom(z=>Math.max(0.12,z/1.15))}><ZoomOutIco/></Btn>
        <span style={{ fontSize:11, fontFamily:"'DM Mono',monospace", color:th.text3, minWidth:38, textAlign:"center" }}>{Math.round(zoom*100)}%</span>
        <Btn th={th} accent={accent} onClick={()=>setZoom(z=>Math.min(3,z*1.15))}><ZoomInIco/></Btn>

        <div style={{ flex:1 }}/>

        {/* Code import */}
        <Btn th={th} accent={accent} title="Import code as mind map" onClick={()=>setCodeModal(true)}><CodeIco/> Code</Btn>

        <Sep th={th}/>
        <Btn th={th} accent={accent} onClick={exportSVG}><ExportIco/> SVG</Btn>
        <Btn th={th} accent={accent} onClick={exportJSON}><JsonIco/> JSON</Btn>

        <Sep th={th}/>

        {/* Theme pill toggle */}
        <div style={{ display:"flex", alignItems:"center", gap:6 }}>
          <span style={{ fontSize:13 }}>☀️</span>
          <div onClick={toggleTheme} style={{
            width:46, height:24, borderRadius:12, cursor:"pointer", position:"relative",
            background: dark ? "#1a1f2e" : "#e0e7ff",
            border:`1.5px solid ${dark?"rgba(255,255,255,.12)":"rgba(0,0,0,.12)"}`,
            transition:"background .2s",
          }}>
            <div style={{
              position:"absolute", top:2, width:18, height:18, borderRadius:"50%",
              background: dark ? "#4f8ef7" : "#f59e0b",
              left: dark ? 24 : 2,
              transition:"left .2s cubic-bezier(.4,0,.2,1), background .2s",
              boxShadow:"0 1px 4px rgba(0,0,0,.3)",
            }}/>
          </div>
          <span style={{ fontSize:13 }}>🌙</span>
        </div>

        <Sep th={th}/>
        <Btn th={th} accent={accent}
          onClick={()=>setPanelOpen(p=>!p)}
          style={{ color:panelOpen?accent:th.text2, background:panelOpen?`${accent}18`:"transparent", border:panelOpen?`1px solid ${accent}33`:"1px solid transparent" }}>
          <PanelIco/>
        </Btn>
      </div>

      {/* ══ BODY ═════════════════════════════════════════════════════════════ */}
      <div style={{ flex:1, display:"flex", overflow:"hidden", position:"relative" }}>

        {/* ── Maps Drawer ── */}
        {drawerOpen && (
          <>
            <div onClick={()=>setDrawerOpen(false)} style={{ position:"absolute", inset:0, background:"rgba(0,0,0,.35)", zIndex:80, backdropFilter:"blur(2px)" }}/>
            <div style={{
              position:"absolute", left:0, top:0, bottom:0, width:300, zIndex:90,
              background:th.surface, borderRight:`1px solid ${th.border}`,
              display:"flex", flexDirection:"column",
              animation:"slideIn .2s ease",
              boxShadow:"4px 0 24px rgba(0,0,0,.3)",
            }}>
              {/* Drawer header */}
              <div style={{ padding:"16px 16px 12px", borderBottom:`1px solid ${th.border}`, display:"flex", alignItems:"center", justifyContent:"space-between" }}>
                <span style={{ fontFamily:"'DM Serif Display',serif", fontStyle:"italic", fontSize:16, color:accent }}>Maps</span>
                <button onClick={()=>setDrawerOpen(false)} style={{ background:"none", border:"none", cursor:"pointer", color:th.text3, fontSize:18, padding:"2px 6px", borderRadius:6 }}
                  onMouseEnter={e=>e.currentTarget.style.color=th.text}
                  onMouseLeave={e=>e.currentTarget.style.color=th.text3}>×</button>
              </div>

              {/* Search */}
              <div style={{ padding:"10px 12px", borderBottom:`1px solid ${th.border}` }}>
                <div style={{ position:"relative" }}>
                  <svg width={13} height={13} viewBox="0 0 16 16" fill="none" stroke={th.text3} strokeWidth={2} style={{ position:"absolute", left:9, top:"50%", transform:"translateY(-50%)", pointerEvents:"none" }}><circle cx="7" cy="7" r="4"/><line x1="10" y1="10" x2="14" y2="14"/></svg>
                  <input
                    value={mapSearch} onChange={e=>setMapSearch(e.target.value)}
                    placeholder="Search maps…" className="panel-inp"
                    style={{ width:"100%", background:th.surface2, border:`1px solid ${th.border2}`, borderRadius:8, color:th.text, fontFamily:"'DM Sans',sans-serif", fontSize:13, padding:"7px 10px 7px 28px" }}
                  />
                </div>
              </div>

              {/* New map */}
              <div style={{ padding:"10px 12px", borderBottom:`1px solid ${th.border}` }}>
                <button onClick={createNewMap} style={{
                  width:"100%", padding:"8px 12px", borderRadius:9, border:`1.5px dashed ${th.border2}`,
                  background:"transparent", color:th.text2, fontFamily:"'DM Sans',sans-serif",
                  fontSize:13, cursor:"pointer", display:"flex", alignItems:"center", gap:7, justifyContent:"center",
                  transition:"all .15s",
                }} onMouseEnter={e=>{e.currentTarget.style.background=th.surface2;e.currentTarget.style.color=accent;e.currentTarget.style.borderColor=accent}}
                   onMouseLeave={e=>{e.currentTarget.style.background="transparent";e.currentTarget.style.color=th.text2;e.currentTarget.style.borderColor=th.border2}}>
                  <PlusIco/> New Map
                </button>
              </div>

              {/* Map list */}
              <div style={{ flex:1, overflowY:"auto" }}>
                {filteredMaps.length === 0 && (
                  <div style={{ padding:"24px 16px", textAlign:"center", color:th.text3, fontSize:12.5 }}>No maps match your search</div>
                )}
                {filteredMaps.map(m => (
                  <div key={m.id} className="map-card"
                    style={{
                      padding:"11px 14px", cursor:"pointer", position:"relative",
                      borderLeft:`3px solid ${m.id===activeMapId?accent:"transparent"}`,
                      background: m.id===activeMapId?`${accent}0e`:"transparent",
                    }}
                    onClick={() => { if (renamingMapId===m.id) return; switchMap(m.id); }}
                  >
                    {renamingMapId === m.id ? (
                      <input autoFocus value={renameVal} onChange={e=>setRenameVal(e.target.value)}
                        onKeyDown={e=>{ if(e.key==="Enter")commitMapRename(); if(e.key==="Escape")setRenamingMapId(null); }}
                        onBlur={commitMapRename} className="panel-inp"
                        style={{ width:"100%", background:th.surface2, border:`1px solid ${accent}`, borderRadius:6, color:th.text, fontFamily:"'DM Sans',sans-serif", fontSize:13, padding:"4px 8px" }}
                        onClick={e=>e.stopPropagation()}
                      />
                    ) : (
                      <>
                        <div style={{ fontSize:13, fontWeight:500, color:m.id===activeMapId?th.text:th.text, marginBottom:3, paddingRight:28, overflow:"hidden", textOverflow:"ellipsis", whiteSpace:"nowrap" }}>{m.name}</div>
                        <div style={{ fontSize:11, color:th.text3, display:"flex", gap:8 }}>
                          <span>{m.nodeCount} nodes</span>
                          <span>·</span>
                          <span>{relTime(m.updatedAt)}</span>
                        </div>
                        {/* Card menu */}
                        <button className="card-menu-btn" onClick={e=>{ e.stopPropagation(); setCardMenu({id:m.id, x:e.clientX, y:e.clientY}); }}
                          style={{ position:"absolute", right:10, top:"50%", transform:"translateY(-50%)", background:"none", border:"none", cursor:"pointer", color:th.text3, padding:"4px 6px", borderRadius:5, fontSize:15 }}
                          onMouseEnter={e=>e.currentTarget.style.color=th.text}
                          onMouseLeave={e=>e.currentTarget.style.color=th.text3}>⋯</button>
                      </>
                    )}
                  </div>
                ))}
              </div>
            </div>
          </>
        )}

        {/* ── Canvas ── */}
        <div style={{ flex:1, position:"relative", overflow:"hidden", cursor: groupMode?"crosshair":"default" }}>
          <GridCanvas dark={dark} pan={pan} zoom={zoom}/>

          <svg ref={svgRef} style={{ position:"absolute", inset:0, width:"100%", height:"100%", overflow:"visible" }} onMouseDown={onBgDown}>
            <g style={{ transform:`translate(calc(50% + ${pan.x}px), calc(50% + ${pan.y}px)) scale(${zoom})`, transformOrigin:"0 0" }}>

              {/* ── Group boxes (layer behind links and nodes) ── */}
              {groups.map(g => {
                const bounds = groupBounds(g.nodeIds, nodes);
                if (!bounds) return null;
                const isSel  = g.id === selectedGroupId;
                const c      = palHex(branchColor(nodes, g.nodeIds[0]) || "blue");
                const tabW   = Math.min(120, Math.max(70, g.label.length * 7.5 + 20));

                return (
                  <g key={`grp-${g.id}`}
                    onClick={e => { e.stopPropagation(); dispatch({type:"SELECT_GROUP",id:g.id}); }}
                    onDoubleClick={e => { e.stopPropagation(); startEditGroup(g.id); }}
                    style={{ cursor:"pointer" }}
                  >
                    {/* Box */}
                    <rect
                      x={bounds.x} y={bounds.y} width={bounds.w} height={bounds.h} rx={14}
                      fill={th.groupFill(branchColor(nodes,g.nodeIds[0])||"blue")}
                      stroke={isSel ? c : th.groupStroke(branchColor(nodes,g.nodeIds[0])||"blue")}
                      strokeWidth={isSel ? 2 : 1.5}
                      strokeDasharray={isSel ? "6 3" : "none"}
                      className="group-rect"
                    />
                    {/* Label tab */}
                    <rect x={bounds.x + 12} y={bounds.y - 11} width={tabW} height={22} rx={7}
                      fill={dark ? th.surface2 : "#fff"}
                      stroke={isSel ? c : th.groupStroke(branchColor(nodes,g.nodeIds[0])||"blue")}
                      strokeWidth={isSel ? 2 : 1.5}
                    />
                    {/* Label text */}
                    {editingGroupId === g.id ? (
                      <foreignObject x={bounds.x+14} y={bounds.y-12} width={tabW-4} height={24}>
                        <input ref={editGrpRef} value={editVal} onChange={e=>setEditVal(e.target.value)}
                          onKeyDown={e=>{ if(e.key==="Enter"||e.key==="Escape"){e.preventDefault();commitEditGroup();} }}
                          onBlur={commitEditGroup}
                          style={{ width:"100%", height:"100%", background:"transparent", border:"none", outline:"none", fontSize:11, fontFamily:"'DM Sans',sans-serif", fontWeight:600, color:c, padding:"0 4px", textAlign:"center" }}
                        />
                      </foreignObject>
                    ) : (
                      <text x={bounds.x + 12 + tabW/2} y={bounds.y + 1}
                        textAnchor="middle" dominantBaseline="middle"
                        fill={c} fontSize={11} fontFamily="'DM Sans',sans-serif" fontWeight={600}
                        style={{ pointerEvents:"none", userSelect:"none" }}
                      >{g.label}</text>
                    )}
                    {/* Collapse arrow */}
                    <text x={bounds.x + 14 + tabW - 12} y={bounds.y + 1}
                      textAnchor="middle" dominantBaseline="middle"
                      fill={c} fontSize={9} style={{ cursor:"pointer" }}
                      onClick={e=>{ e.stopPropagation(); dispatch({type:"TOGGLE_GROUP_COLLAPSE",id:g.id}); }}
                    >{g.collapsed?"▸":"▾"}</text>
                  </g>
                );
              })}

              {/* ── Links ── */}
              {visLinks.map(n => {
                const p   = getNode(nodes, n.parentId);
                const c   = palHex(branchColor(nodes, n.id));
                const sel = n.id===selectedId || p.id===selectedId;
                const cpx = (p.x + n.x) / 2;
                return (
                  <path key={`l${n.id}`}
                    d={`M${p.x},${p.y} C${cpx},${p.y} ${cpx},${n.y} ${n.x},${n.y}`}
                    fill="none" stroke={c} strokeWidth={sel?2.5:1.8} opacity={sel?.9:.42}
                    style={{ transition:"opacity .2s,stroke-width .2s", pointerEvents:"none" }}
                  />
                );
              })}

              {/* ── Nodes ── */}
              {visNodes.map(n => {
                const isRoot = !n.parentId;
                const isSel  = n.id === selectedId;
                const c      = palHex(branchColor(nodes, n.id));
                const w      = Math.max(NODE_W, n.label.length * 8.5 + 34);
                const hasKids= getChildren(nodes, n.id).length > 0;

                return (
                  <g key={n.id}
                    style={{ transform:`translate(${n.x}px,${n.y}px)`, cursor:"grab" }}
                    onMouseDown={makeNodeDown(n.id)}
                    onDoubleClick={e=>{ e.stopPropagation(); startEdit(n.id); }}
                    onContextMenu={e=>{ e.preventDefault(); e.stopPropagation(); dispatch({type:"SELECT",id:n.id}); setCtxMenu({id:n.id,x:e.clientX,y:e.clientY}); }}
                  >
                    {isSel && <rect x={-w/2-5} y={-NODE_H/2-5} width={w+10} height={NODE_H+10} rx={15} fill={`${c}14`}/>}
                    <rect x={-w/2} y={-NODE_H/2} width={w} height={NODE_H} rx={10}
                      fill={isRoot?`${c}20`:th.nodeBg(branchColor(nodes,n.id))}
                      stroke={isSel?c:(dark?"rgba(255,255,255,.09)":"rgba(0,0,0,.09)")}
                      strokeWidth={isSel?2:1}
                      style={{ filter:isSel?`drop-shadow(0 0 10px ${c}66)`:"drop-shadow(0 3px 10px rgba(0,0,0,.28))" }}
                    />
                    {isSel && (
                      <rect x={-w/2-2} y={-NODE_H/2-2} width={w+4} height={NODE_H+4} rx={12}
                        fill="none" stroke="rgba(255,255,255,.28)" strokeWidth={1.5}
                        strokeDasharray="5 4"
                        style={{ animation:"selDash 5s linear infinite", pointerEvents:"none" }}
                      />
                    )}
                    <text x={0} y={1} textAnchor="middle" dominantBaseline="middle"
                      fill={isRoot?c:th.nodeText} fontSize={isRoot?15:13}
                      fontFamily={isRoot?"'DM Serif Display',serif":"'DM Sans',sans-serif"}
                      fontStyle={isRoot?"italic":"normal"} fontWeight={isRoot?"400":"500"}
                      style={{ pointerEvents:"none", userSelect:"none" }}
                    >{n.label}</text>
                    {hasKids && (
                      <circle className="cdot" cx={w/2+9} cy={0} r={5.5}
                        fill={n.collapsed?c:`${c}88`}
                        stroke={dark?"#0c0e14":"#eef1f8"} strokeWidth={1.5} style={{ cursor:"pointer" }}
                        onClick={e=>{ e.stopPropagation(); dispatch({type:"TOGGLE_COLLAPSE",id:n.id}); }}
                      />
                    )}
                  </g>
                );
              })}
            </g>

            {/* ── Lasso rectangle (screen coords, outside the world transform) ── */}
            {lasso && (
              <rect
                x={Math.min(lasso.sx,lasso.ex)} y={Math.min(lasso.sy,lasso.ey)}
                width={Math.abs(lasso.ex-lasso.sx)} height={Math.abs(lasso.ey-lasso.sy)}
                fill={`${accent}10`} stroke={accent} strokeWidth={1.5} strokeDasharray="5 3"
                style={{ pointerEvents:"none" }}
              />
            )}
          </svg>

          {/* ── Inline node rename overlay ── */}
          {editNode && editOverlay && (
            <input ref={editRef} value={editVal} onChange={e=>setEditVal(e.target.value)}
              onKeyDown={e=>{ if(e.key==="Enter"||e.key==="Escape"){e.preventDefault();commitEdit();} }}
              onBlur={commitEdit} className="panel-inp"
              style={{
                position:"absolute", left:editOverlay.left, top:editOverlay.top, width:editOverlay.width+24,
                background:th.surface2, border:`2px solid ${accent}`, borderRadius:10, color:th.text,
                fontFamily:"'DM Sans',sans-serif", fontSize:13, fontWeight:500, padding:"5px 12px", textAlign:"center",
                zIndex:200, boxShadow:`0 0 0 4px ${accent}22,0 8px 24px rgba(0,0,0,.3)`,
              }}
            />
          )}

          {/* ── Context menu ── */}
          {ctxMenu && (
            <>
              <div style={{ position:"fixed", inset:0, zIndex:149 }} onClick={()=>setCtxMenu(null)}/>
              <CtxMenu x={ctxMenu.x} y={ctxMenu.y} th={th}
                onChild={()=>{ dispatch({type:"ADD_CHILD",parentId:ctxMenu.id}); setCtxMenu(null); }}
                onSibling={()=>{ dispatch({type:"ADD_SIBLING",id:ctxMenu.id}); setCtxMenu(null); }}
                onRename={()=>{ startEdit(ctxMenu.id); setCtxMenu(null); }}
                onCollapse={()=>{ dispatch({type:"TOGGLE_COLLAPSE",id:ctxMenu.id}); setCtxMenu(null); }}
                onDelete={()=>{ dispatch({type:"DELETE",id:ctxMenu.id}); setCtxMenu(null); }}
              />
            </>
          )}

          {/* Group mode hint */}
          {groupMode && !lasso && (
            <div style={{ position:"absolute", bottom:16, left:"50%", transform:"translateX(-50%)", background:th.surface2, border:`1px solid ${accent}44`, borderRadius:8, padding:"7px 16px", fontSize:12.5, color:th.text2, pointerEvents:"none", boxShadow:"0 4px 16px rgba(0,0,0,.25)" }}>
              Drag to select nodes and create a group · <span style={{color:th.text3}}>Esc to cancel</span>
            </div>
          )}
        </div>

        {/* ── Side Panel ── */}
        {panelOpen && (
          <div style={{ width:274, background:th.surface, borderLeft:`1px solid ${th.border}`, display:"flex", flexDirection:"column", flexShrink:0 }}>
            <div style={{ padding:"13px 17px 10px", borderBottom:`1px solid ${th.border}`, fontSize:10, fontWeight:700, letterSpacing:".09em", textTransform:"uppercase", color:th.text3 }}>
              {selGroup ? "Group Inspector" : "Node Inspector"}
            </div>

            {/* Group selected */}
            {selGroup && (
              <div style={{ flex:1, display:"flex", flexDirection:"column", overflowY:"auto" }}>
                <PS label="Group Name" th={th}>
                  <input className="panel-inp" value={selGroup.label}
                    onChange={e=>dispatch({type:"RENAME_GROUP",id:selectedGroupId,label:e.target.value||"Group"})}
                    style={iSty(th)}/>
                </PS>
                <PS label="Members" th={th}>
                  {selGroup.nodeIds.map(id => {
                    const n = getNode(nodes, id);
                    return n ? (
                      <div key={id} style={{ fontSize:12.5, color:th.text2, padding:"3px 0", display:"flex", alignItems:"center", gap:6 }}>
                        <div style={{ width:8, height:8, borderRadius:"50%", background:palHex(branchColor(nodes,id)), flexShrink:0 }}/>
                        {n.label}
                      </div>
                    ) : null;
                  })}
                </PS>
                <PS label="Actions" th={th}>
                  <button onClick={()=>dispatch({type:"DELETE_GROUP",id:selectedGroupId})}
                    style={{ background:"rgba(244,63,94,.1)", border:"1px solid rgba(244,63,94,.25)", borderRadius:7, color:"#f43f5e", padding:"7px 14px", fontSize:12.5, cursor:"pointer", fontFamily:"'DM Sans',sans-serif" }}>
                    Delete Box (keep nodes)
                  </button>
                </PS>
              </div>
            )}

            {/* Node selected */}
            {!selGroup && selNode && (
              <div style={{ flex:1, overflowY:"auto", display:"flex", flexDirection:"column" }}>
                <PS label="Label" th={th}>
                  <input className="panel-inp" value={selNode.label}
                    onChange={e=>dispatch({type:"RENAME",id:selectedId,label:e.target.value||"Node"})}
                    style={iSty(th)}/>
                </PS>
                <PS label="Notes" th={th}>
                  <textarea className="panel-inp" value={selNode.notes}
                    onChange={e=>dispatch({type:"SET_NOTES",id:selectedId,notes:e.target.value})}
                    rows={4} placeholder="Add notes…" style={{...iSty(th),resize:"none",lineHeight:1.5}}/>
                </PS>
                <PS label="Branch Color" th={th}>
                  <div style={{ display:"flex", gap:7, flexWrap:"wrap" }}>
                    {PALETTE.map(c => (
                      <div key={c.id} className="swatch"
                        onClick={()=>dispatch({type:"SET_COLOR",id:selectedId,color:c.id})}
                        style={{ width:28, height:28, borderRadius:8, background:c.hex, border:selNode.color===c.id?"2.5px solid #fff":"2px solid transparent", boxShadow:selNode.color===c.id?`0 0 0 3px ${c.hex}55`:"none" }}
                      />
                    ))}
                  </div>
                </PS>
                <PS label="Info" th={th}>
                  {[["ID",selNode.id],["Depth",getDepth(nodes,selectedId)],["Children",getChildren(nodes,selectedId).length]].map(([k,v])=>(
                    <div key={k} style={{ display:"flex", justifyContent:"space-between", padding:"2px 0", fontSize:12 }}>
                      <span style={{color:th.text3}}>{k}</span>
                      <span style={{color:th.text2,fontFamily:"'DM Mono',monospace",fontSize:11.5}}>{v}</span>
                    </div>
                  ))}
                  {selNode.notes && (
                    <div style={{ marginTop:8, padding:"7px 10px", background:th.surface2, borderRadius:7, fontSize:12, color:th.text2, lineHeight:1.5, borderLeft:`3px solid ${accent}` }}>
                      {selNode.notes}
                    </div>
                  )}
                </PS>
              </div>
            )}

            {/* Nothing selected */}
            {!selGroup && !selNode && (
              <div style={{ flex:1, display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", color:th.text3, gap:10 }}>
                <svg width={38} height={38} viewBox="0 0 38 38" fill="none" stroke="currentColor" strokeWidth={1.2}><circle cx="19" cy="19" r="14"/><circle cx="19" cy="19" r="5"/><line x1="19" y1="5" x2="19" y2="14"/><line x1="19" y1="24" x2="19" y2="33"/><line x1="5" y1="19" x2="14" y2="19"/><line x1="24" y1="19" x2="33" y2="19"/></svg>
                <span style={{fontSize:12.5}}>Select a node or group</span>
              </div>
            )}

            {/* Shortcuts */}
            <div style={{ borderTop:`1px solid ${th.border}`, padding:"12px 16px", flexShrink:0 }}>
              <div style={{ fontSize:10, fontWeight:700, letterSpacing:".09em", textTransform:"uppercase", color:th.text3, marginBottom:9 }}>Shortcuts</div>
              {[["Add Child","Tab"],["Sibling","Enter"],["Delete","Del"],["Rename","F2"],["Group mode","G"],["Undo/Redo","⌘Z/⌘Y"],["Fit","⌘⇧F"]].map(([l,k])=>(
                <div key={l} style={{ display:"flex", justifyContent:"space-between", alignItems:"center", padding:"3px 0", fontSize:11.5 }}>
                  <span style={{color:th.text2}}>{l}</span>
                  <kbd style={{ fontFamily:"'DM Mono',monospace", fontSize:10, background:th.surface3, border:`1px solid ${th.border2}`, borderRadius:4, padding:"2px 6px", color:th.text }}>{k}</kbd>
                </div>
              ))}
            </div>
          </div>
        )}
      </div>

      {/* ══ STATUS BAR ═══════════════════════════════════════════════════════ */}
      <div style={{ height:33, background:th.surface, borderTop:`1px solid ${th.border}`, display:"flex", alignItems:"center", padding:"0 16px", gap:20, fontSize:11.5, fontFamily:"'DM Mono',monospace", color:th.text3, flexShrink:0 }}>
        <div style={{ display:"flex", alignItems:"center", gap:6 }}>
          <div style={{ width:6, height:6, borderRadius:"50%", background:accent }}/>
          <span>Nodes: <span style={{color:th.text2}}>{nodes.length}</span></span>
        </div>
        <span>Depth: <span style={{color:th.text2}}>{treeMaxDepth(nodes)}</span></span>
        <span>Selected: <span style={{color:th.text2}}>{selNode?.label ?? selGroup?.label ?? "—"}</span></span>
        <span>Zoom: <span style={{color:th.text2}}>{Math.round(zoom*100)}%</span></span>
        <div style={{ flex:1 }}/>
        {/* Save status */}
        <div style={{ display:"flex", alignItems:"center", gap:5 }}>
          <div style={{
            width:6, height:6, borderRadius:"50%",
            background: saveStatus==="saved" ? "#14b8a6" : saveStatus==="saving" ? "#f59e0b" : "#f59e0b",
            animation: saveStatus==="pending" ? "pulse 1.2s ease-in-out infinite" : "none",
          }}/>
          <span style={{ color:th.text3 }}>
            {saveStatus==="saved" ? "Saved" : saveStatus==="saving" ? "Saving…" : "Unsaved changes"}
          </span>
        </div>
        <span style={{color:th.text3}}>✦ MindMap Pro</span>
      </div>

      {/* ══ CODE IMPORT MODAL ════════════════════════════════════════════════ */}
      {codeModal && (
        <div style={{ position:"fixed", inset:0, background:"rgba(0,0,0,.6)", zIndex:500, display:"flex", alignItems:"center", justifyContent:"center", backdropFilter:"blur(4px)" }}
          onClick={e=>{ if(e.target===e.currentTarget){setCodeModal(false);setCodeText("");setDetectedLang("—");} }}>
          <div style={{ background:th.surface, border:`1px solid ${th.border2}`, borderRadius:16, padding:28, width:560, maxWidth:"95vw", boxShadow:"0 24px 60px rgba(0,0,0,.5)", animation:"fadeUp .2s ease" }}>
            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:20 }}>
              <div>
                <div style={{ fontFamily:"'DM Serif Display',serif", fontStyle:"italic", fontSize:18, color:accent }}>Import Code</div>
                <div style={{ fontSize:12, color:th.text3, marginTop:3 }}>Paste any code — Claude will map its structure</div>
              </div>
              <button onClick={()=>{setCodeModal(false);setCodeText("");setDetectedLang("—");}}
                style={{ background:"none", border:"none", cursor:"pointer", color:th.text3, fontSize:22, padding:"2px 8px", borderRadius:6 }}
                onMouseEnter={e=>e.currentTarget.style.color=th.text}
                onMouseLeave={e=>e.currentTarget.style.color=th.text3}>×</button>
            </div>

            <textarea value={codeText} onChange={e=>setCodeText(e.target.value)}
              placeholder="Paste your code here…" className="panel-inp"
              style={{ width:"100%", height:240, background:th.surface2, border:`1px solid ${th.border2}`, borderRadius:10, color:th.text, fontFamily:"'DM Mono',monospace", fontSize:12.5, padding:"12px 14px", resize:"none", lineHeight:1.6, marginBottom:16 }}
            />

            <div style={{ display:"flex", alignItems:"center", justifyContent:"space-between" }}>
              <div style={{ fontSize:12.5, color:th.text3 }}>
                Detected: <span style={{ color: detectedLang==="—" ? th.text3 : accent, fontWeight:500 }}>{detectedLang}</span>
              </div>
              <div style={{ display:"flex", gap:10 }}>
                <button onClick={()=>{setCodeModal(false);setCodeText("");setDetectedLang("—");}}
                  style={{ padding:"8px 18px", borderRadius:9, border:`1px solid ${th.border2}`, background:"transparent", color:th.text2, fontFamily:"'DM Sans',sans-serif", fontSize:13, cursor:"pointer" }}>
                  Cancel
                </button>
                <button onClick={handleCodeImport} disabled={!codeText.trim() || codeLoading}
                  style={{ padding:"8px 20px", borderRadius:9, border:"none", background:codeText.trim()&&!codeLoading?accent:"#2a2f44", color:codeText.trim()&&!codeLoading?"#fff":th.text3, fontFamily:"'DM Sans',sans-serif", fontSize:13, fontWeight:500, cursor:codeText.trim()&&!codeLoading?"pointer":"not-allowed", display:"flex", alignItems:"center", gap:8, transition:"background .15s" }}>
                  {codeLoading ? (
                    <><svg width={14} height={14} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth={2.5} style={{ animation:"spin 1s linear infinite" }}><circle cx="12" cy="12" r="10" strokeOpacity=".25"/><path d="M12 2a10 10 0 0 1 10 10" /></svg> Analyzing…</>
                  ) : <>Generate Map →</>}
                </button>
              </div>
            </div>
          </div>
        </div>
      )}

      {/* ══ CARD CONTEXT MENU ════════════════════════════════════════════════ */}
      {cardMenu && (
        <>
          <div style={{ position:"fixed", inset:0, zIndex:299 }} onClick={()=>setCardMenu(null)}/>
          <div style={{ position:"fixed", left:Math.min(cardMenu.x, window.innerWidth-160), top:Math.min(cardMenu.y, window.innerHeight-160), zIndex:300, background:th.surface2, border:`1px solid ${th.border2}`, borderRadius:10, padding:5, minWidth:150, boxShadow:"0 8px 28px rgba(0,0,0,.35)", animation:"fadeUp .15s ease" }}>
            {[
              { label:"Rename", onClick:()=>{ setRenamingMapId(cardMenu.id); setRenameVal(maps.find(m=>m.id===cardMenu.id)?.name||""); setCardMenu(null); } },
              { label:"Duplicate", onClick:()=>{ duplicateMap(cardMenu.id); setCardMenu(null); } },
              { sep:true },
              { label:"Delete", danger:true, onClick:()=>{ deleteMap(cardMenu.id); setCardMenu(null); } },
            ].map((r,i) => r.sep
              ? <div key={i} style={{ height:1, background:th.border2, margin:"4px 0" }}/>
              : <div key={r.label} className={r.danger?"ctx-danger":"ctx-item"} onClick={r.onClick}
                  style={{ padding:"8px 12px", borderRadius:7, fontSize:13, cursor:"pointer", color:r.danger?"#f43f5e":th.text2 }}>{r.label}</div>
            )}
          </div>
        </>
      )}

      {/* Toast */}
      <div style={{
        position:"fixed", bottom:44, left:"50%",
        transform:`translateX(-50%) translateY(${toast?0:8}px)`,
        background:th.surface2, border:`1px solid ${th.border2}`, borderRadius:8,
        padding:"7px 18px", fontSize:12.5, color:th.text2,
        opacity:toast?1:0, pointerEvents:"none",
        transition:"opacity .2s,transform .2s",
        boxShadow:"0 6px 20px rgba(0,0,0,.3)", zIndex:999,
      }}>{toast}</div>

      <style>{`@keyframes spin{to{transform:rotate(360deg)}}`}</style>
    </div>
  );
}

// ════════════════════════════════════════════════════════════════════════════
// SUB-COMPONENTS
// ════════════════════════════════════════════════════════════════════════════

function GridCanvas({ dark, pan, zoom }) {
  const ref = useRef(null);
  useEffect(() => {
    const el = ref.current; if (!el) return;
    const draw = () => {
      const p = el.parentElement; if (!p) return;
      el.width = p.clientWidth; el.height = p.clientHeight;
      const ctx = el.getContext("2d");
      ctx.clearRect(0, 0, el.width, el.height);
      const step = 28 * zoom;
      const ox = (el.width  / 2 + pan.x) % step;
      const oy = (el.height / 2 + pan.y) % step;
      ctx.strokeStyle = dark ? "rgba(255,255,255,.03)" : "rgba(0,0,0,.036)";
      ctx.lineWidth = 1;
      for (let x = ox; x < el.width;  x += step) { ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,el.height); ctx.stroke(); }
      for (let y = oy; y < el.height; y += step) { ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(el.width,y);  ctx.stroke(); }
    };
    draw();
    const ro = new ResizeObserver(draw); ro.observe(el.parentElement);
    return () => ro.disconnect();
  }, [dark, pan, zoom]);
  return <canvas ref={ref} style={{ position:"absolute", inset:0, width:"100%", height:"100%", pointerEvents:"none" }}/>;
}

function Btn({ children, onClick, title, primary, th, accent, style: s={} }) {
  return (
    <button className="tb" onClick={onClick} title={title} style={{
      display:"flex", alignItems:"center", gap:5, padding:"5px 9px",
      borderRadius:8, border:"1px solid transparent",
      background:primary?accent:"transparent",
      color:primary?"#fff":th.text2,
      fontFamily:"'DM Sans',sans-serif", fontSize:12.5, fontWeight:500,
      cursor:"pointer", whiteSpace:"nowrap", ...s,
    }}>{children}</button>
  );
}

function Sep({ th }) {
  return <div style={{ width:1, height:26, background:th.border2, margin:"0 4px", flexShrink:0 }}/>;
}

function PS({ label, th, children }) {
  return (
    <div style={{ padding:"12px 16px", borderBottom:`1px solid ${th.border}` }}>
      <div style={{ fontSize:10, fontWeight:700, letterSpacing:".09em", textTransform:"uppercase", color:th.text3, marginBottom:8 }}>{label}</div>
      {children}
    </div>
  );
}

function iSty(th) {
  return { width:"100%", background:th.surface2, border:`1px solid ${th.border2}`, borderRadius:8, color:th.text, fontFamily:"'DM Sans',sans-serif", fontSize:13, padding:"8px 10px" };
}

function CtxMenu({ x, y, th, onChild, onSibling, onRename, onCollapse, onDelete }) {
  const left = Math.min(x, window.innerWidth-192);
  const top  = Math.min(y, window.innerHeight-220);
  return (
    <div style={{ position:"fixed", left, top, zIndex:200, background:th.surface2, border:`1px solid ${th.border2}`, borderRadius:11, padding:5, minWidth:184, boxShadow:"0 12px 36px rgba(0,0,0,.4)", animation:"fadeUp .15s ease" }}>
      {[
        { label:"Add Child",       icon:<PlusIco/>,     onClick:onChild },
        { label:"Add Sibling",     icon:<MinusIco/>,    onClick:onSibling },
        { label:"Rename",          icon:<EditIco/>,     onClick:onRename },
        { label:"Toggle Collapse", icon:<CollapseIco/>, onClick:onCollapse },
        { sep:true },
        { label:"Delete",          icon:<TrashIco/>,    onClick:onDelete, danger:true },
      ].map((r,i) => r.sep
        ? <div key={i} style={{ height:1, background:th.border2, margin:"4px 0" }}/>
        : <div key={r.label} className={r.danger?"ctx-danger":"ctx-item"} onClick={r.onClick}
            style={{ display:"flex", alignItems:"center", gap:9, padding:"8px 10px", borderRadius:7, fontSize:13, cursor:"pointer", color:r.danger?"#f43f5e":th.text2 }}>
            {r.icon}{r.label}
          </div>
      )}
    </div>
  );
}

// ════════════════════════════════════════════════════════════════════════════
// ICONS
// ════════════════════════════════════════════════════════════════════════════
const I = (paths, sw=1.9) => (
  <svg width={13} height={13} viewBox="0 0 16 16" fill="none" stroke="currentColor" strokeWidth={sw} strokeLinecap="round" strokeLinejoin="round">
    {[].concat(paths).map((d,i)=><path key={i} d={d}/>)}
  </svg>
);
const PlusIco    = () => I(["M8 3v10","M3 8h10"]);
const MinusIco   = () => I("M3 8h10");
const TrashIco   = () => I(["M3 5h10","M5.5 5V3.5h5V5","M4.5 5l1 9h5l1-9"]);
const UndoIco    = () => I(["M3.5 8C3.5 5 6 2.5 9 2.5C12 2.5 13.5 5 13.5 8s-2 5.5-5 5.5H4","M3.5 10.5v-2.5h2"]);
const RedoIco    = () => I(["M12.5 8C12.5 5 10 2.5 7 2.5C4 2.5 2.5 5 2.5 8s2 5.5 5 5.5H12","M12.5 10.5v-2.5h-2"]);
const FitIco     = () => I(["M2 2h5v2H4v3H2z","M9 2h5v5h-2V4H9z","M14 9v5H9v-2h3v-3z","M2 9h2v3h3v2H2z"]);
const ZoomInIco  = () => I(["M7 3a4 4 0 100 8A4 4 0 007 3z","M7 5v4","M5 7h4","M11 11l3 3"]);
const ZoomOutIco = () => I(["M7 3a4 4 0 100 8A4 4 0 007 3z","M5 7h4","M11 11l3 3"]);
const ExportIco  = () => I(["M2 2h12v12H2z","M5 10.5l2.5-4 2 2.5 1.5-2 2 3.5"]);
const JsonIco    = () => I(["M4 3C2 3 2 5.5 4 5.5C2 5.5 2 8 4 8","M12 3C14 3 14 5.5 12 5.5C14 5.5 14 8 12 8","M7 5.5h2"]);
const PanelIco   = () => I(["M1.5 2h13v12h-13z","M10.5 2v12"]);
const EditIco    = () => I(["M3 13l9-9 2 2-9 9H3z","M11 5l-2-2"]);
const CollapseIco= () => I("M4 8h8");
const GroupIco   = () => I(["M2 5h12v8H2z","M5 2v3","M11 2v3"],1.8);
const MapsIco    = () => I(["M2 3l4 1.5 4-1.5 4 1.5v10l-4-1.5-4 1.5-4-1.5z","M6 4.5v10","M10 3v10"],1.8);
const CodeIco    = () => I(["M5 4l-3 4 3 4","M11 4l3 4-3 4","M8 2l-2 12"],1.8);
