
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Armstrong | Outreach Engine</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap');

        :root {
            --primary-color: #4f46e5; /* Deep Indigo */
            --primary-light: #e0e7ff;
            --primary-hover: #4338ca;
            --background-color: #f8fafc; 
            --sidebar-bg: #ffffff;
            --text-color: #0f172a;
            --text-secondary: #64748b;
            --text-tertiary: #94a3b8;
            --border-color: #e2e8f0;
            --glass-bg: rgba(255, 255, 255, 0.95);
            --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
            --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
            
            --level-0-indent: 12px;
            --level-1-indent: 28px;
            --level-2-indent: 44px;
            --level-3-indent: 64px;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--background-color);
            color: var(--text-color);
            margin: 0;
            height: 100vh;
            overflow: hidden;
            -webkit-font-smoothing: antialiased;
        }

        /* CUSTOM SCROLLBAR */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: transparent; 
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1; 
            border-radius: 4px;
            border: 2px solid transparent;
            background-clip: content-box;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
            border: 2px solid transparent;
            background-clip: content-box;
        }

        /* UTILS */
        .glass-pane {
            background: var(--glass-bg);
            backdrop-filter: blur(8px);
            border-bottom: 1px solid var(--border-color);
        }
        
        .chrome-button {
            padding: 0.5rem 1rem;
            border-radius: 8px;
            font-weight: 500;
            font-size: 0.85rem;
            cursor: pointer;
            border: 1px solid var(--border-color);
            background: white;
            color: var(--text-color);
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 6px;
            box-shadow: var(--shadow-sm);
        }
        .chrome-button:hover:not(:disabled) { transform: translateY(-1px); box-shadow: var(--shadow-md); border-color: #cbd5e1; }
        .chrome-button:active:not(:disabled) { transform: translateY(0); }
        .chrome-button.primary { background: var(--primary-color); color: white; border: 1px solid transparent; }
        .chrome-button.primary:hover:not(:disabled) { background: var(--primary-hover); box-shadow: 0 4px 12px rgba(79, 70, 229, 0.25); }
        .chrome-button.send-btn { background: #10b981; color: white; border: 1px solid transparent; }
        .chrome-button.send-btn:hover:not(:disabled) { background: #059669; box-shadow: 0 4px 12px rgba(16, 185, 129, 0.25); }
        .chrome-button:disabled { opacity: 0.6; cursor: not-allowed; transform: none; box-shadow: none; }

        .icon-button {
            background: transparent;
            border: 1px solid transparent;
            border-radius: 8px;
            padding: 6px;
            cursor: pointer;
            color: var(--text-secondary);
            display: flex; align-items: center; justify-content: center;
            transition: all 0.2s;
        }
        .icon-button:hover { background: rgba(99, 102, 241, 0.08); color: var(--primary-color); }
        .icon-button.active { background: #e0e7ff; color: var(--primary-color); }

        .input-solid {
            background-color: #ffffff !important;
            color: #1e293b !important;
            border: 1px solid #cbd5e1 !important;
            border-radius: 6px;
            font-family: inherit;
        }
        .no-bg-input {
            background: transparent !important;
            border: none !important;
            color: #1e293b !important;
            outline: none;
            min-width: 150px;
            font-family: inherit;
        }

        /* LAYOUT */
        .main-layout {
            display: flex;
            flex-direction: column;
            height: 100vh;
            position: relative;
        }

        /* HEADER */
        .control-bar {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 1.5rem;
            gap: 1.5rem;
            z-index: 50;
            height: 60px;
            background: white;
            flex-shrink: 0;
            border-bottom: 1px solid var(--border-color);
        }

        .control-group { display: flex; align-items: center; gap: 0.75rem; }
        .logo-group { width: auto; min-width: 80px; display: flex; align-items: center; gap: 12px; }
        .inputs-group { flex-grow: 1; display: flex; gap: 0.75rem; max-width: 900px; }
        .actions-group { justify-content: flex-end; }

        .app-logo { font-weight: 800; font-size: 1.25rem; background: linear-gradient(135deg, #4f46e5 0%, #9333ea 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }

        .user-avatar {
            width: 32px; height: 32px;
            border-radius: 50%;
            background: linear-gradient(135deg, #4f46e5, #818cf8);
            color: white; font-weight: 600; font-size: 0.85rem;
            display: flex; align-items: center; justify-content: center;
            cursor: pointer; border: 2px solid white;
            box-shadow: 0 0 0 1px var(--border-color);
        }
        .sign-out-icon { background: none; border: none; cursor: pointer; color: var(--text-secondary); padding: 5px; transition: color 0.2s; }
        .sign-out-icon:hover { color: #ef4444; }

        /* BODY LAYOUT: SIDEBAR + RESULTS */
        .app-body {
            display: flex;
            flex: 1;
            overflow: hidden;
            background: #fff;
        }

        /* --- BEAUTIFIED SIDEBAR STYLES --- */
        .location-sidebar {
            width: 320px;
            background: var(--sidebar-bg);
            border-right: 1px solid var(--border-color);
            display: flex;
            flex-direction: column;
            overflow: hidden;
            flex-shrink: 0;
            position: relative;
        }
        
        .sidebar-header {
            padding: 16px 20px;
            background: #ffffff;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
            flex-shrink: 0;
        }
        .sidebar-title {
            display: flex; align-items: center; gap: 10px;
            font-weight: 700; color: var(--text-color); font-size: 0.95rem;
            letter-spacing: -0.01em;
        }
        .sidebar-badge {
            font-size: 0.7rem; font-weight: 600;
            background: var(--primary-light); color: var(--primary-color);
            padding: 2px 8px; border-radius: 99px;
        }

        .tree-container {
            flex: 1;
            overflow-y: auto;
            padding: 8px 0;
        }

        .tree-node-wrapper {
            display: flex;
            flex-direction: column;
        }

        /* Modern Tree Row - Clean & Professional */
        .tree-row {
            display: flex;
            align-items: center;
            padding: 4px 12px 4px 0; /* Reduced vertical padding for tighter list */
            cursor: pointer;
            transition: background 0.1s ease, color 0.1s ease;
            color: var(--text-secondary);
            user-select: none;
            position: relative;
            min-height: 30px;
        }

        /* Hover & Active States */
        .tree-row:hover {
            background-color: #f1f5f9;
            color: var(--text-color);
        }
        .tree-row.active-selection {
            background-color: #f5f3ff; /* Very Light Indigo */
            color: var(--primary-color);
        }
        /* When expanded but not selected */
        .tree-row.expanded:not(.active-selection) {
            background-color: #f8fafc;
        }
        
        /* Indentation & Typography by Level */
        .tree-row.level-0 {
            padding-left: var(--level-0-indent);
        }
        .tree-row.level-0 .tree-label {
            font-weight: 700;
            font-size: 0.9rem;
            color: var(--text-color);
        }
        
        .tree-row.level-1 {
            padding-left: var(--level-1-indent);
        }
        .tree-row.level-1 .tree-label {
            font-weight: 600;
            font-size: 0.85rem;
            color: #475569;
        }
        
        .tree-row.level-2 {
            padding-left: var(--level-2-indent);
        }
        .tree-row.level-2 .tree-label {
            font-weight: 500;
            font-size: 0.85rem;
            color: #64748b;
        }
        
        .tree-row.level-3 {
            padding-left: var(--level-3-indent);
        }
        .tree-row.level-3 .tree-label {
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.8rem;
            color: #94a3b8;
        }
        .tree-row.level-3:hover .tree-label {
            color: var(--primary-color);
        }
        
        .tree-label {
            flex: 1;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            margin-left: 6px;
        }

        /* Tree Icons */
        .tree-chevron {
            width: 20px;
            height: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 4px;
            color: #cbd5e1;
            transition: all 0.2s;
            margin-right: 2px;
            flex-shrink: 0;
        }
        .tree-chevron:hover {
            background-color: rgba(0,0,0,0.04);
            color: var(--text-secondary);
        }
        .tree-chevron.rotated svg {
            transform: rotate(90deg);
        }
        .tree-chevron svg {
            width: 14px;
            height: 14px;
            transition: transform 0.2s;
        }
        .tree-chevron.hidden {
            visibility: hidden;
            pointer-events: none;
        }

        /* Checkboxes */
        .tree-checkbox {
            width: 18px;
            height: 18px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #cbd5e1;
            transition: color 0.2s;
            margin-right: 4px;
            flex-shrink: 0;
        }
        .tree-checkbox:hover {
            color: #94a3b8;
        }
        .tree-checkbox.checked, .tree-checkbox.indeterminate {
            color: var(--primary-color);
        }
        .checkbox-svg {
            width: 16px; 
            height: 16px;
        }

        /* Count Badge */
        .tree-count {
            font-size: 0.65rem;
            font-weight: 600;
            padding: 1px 6px;
            border-radius: 4px;
            background-color: #f1f5f9;
            color: #94a3b8;
            min-width: 16px;
            text-align: center;
            margin-left: auto;
            margin-right: 8px;
        }
        .tree-row:hover .tree-count {
            background-color: #e2e8f0;
            color: #64748b;
        }

        .tree-children {
            position: relative;
        }

        /* RESULTS CONTENT */
        .results-content {
            flex: 1;
            overflow-y: auto;
            background: #f8fafc;
            position: relative;
            padding: 1.5rem;
        }

        /* DROPDOWN & ACCORDION */
        .accordion-panel {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.35s cubic-bezier(0.4, 0, 0.2, 1), opacity 0.3s;
            opacity: 0;
            background: rgba(255, 255, 255, 0.98);
            backdrop-filter: blur(12px);
            border-bottom: 1px solid var(--border-color);
            position: absolute;
            top: 60px;
            left: 0; right: 0;
            z-index: 40;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
        }
        .accordion-panel.open { max-height: 600px; opacity: 1; overflow-y: auto; }
        .accordion-content { padding: 2rem; max-width: 1200px; margin: 0 auto; }

        .saved-searches-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
            gap: 1.25rem;
        }
        
        .snapshot-card {
            background: white;
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 1.25rem;
            cursor: pointer;
            position: relative;
            transition: all 0.2s ease;
            box-shadow: var(--shadow-sm);
        }
        .snapshot-card:hover { transform: translateY(-3px); box-shadow: var(--shadow-md); border-color: #a5b4fc; }
        .save-cat { font-weight: 700; font-size: 1.05rem; color: var(--text-color); margin-bottom: 0.5rem; padding-right: 20px;}
        .snapshot-meta { font-size: 0.8rem; color: var(--text-secondary); display: flex; justify-content: space-between; margin: 8px 0; }
        .snapshot-badges { display: flex; gap: 6px; margin-top: 10px; }
        .badge { font-size: 0.7rem; padding: 3px 8px; border-radius: 6px; font-weight: 600; letter-spacing: 0.02em; }
        .badge.location { background: #f0f9ff; color: #0284c7; border: 1px solid #bae6fd; }
        .badge.business { background: #f0fdf4; color: #16a34a; border: 1px solid #bbf7d0; }
        .delete-save-btn { position: absolute; top: 10px; right: 10px; background: none; border: none; color: #cbd5e1; cursor: pointer; padding: 4px; border-radius: 4px;}
        .delete-save-btn:hover { color: #ef4444; background: #fef2f2; }

        /* MULTI-SELECT INPUT */
        .custom-multiselect {
            position: relative;
            background: white;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 4px 6px;
            min-height: 42px;
            display: flex;
            align-items: center;
            transition: border-color 0.2s, box-shadow 0.2s;
        }
        .custom-multiselect:focus-within { border-color: var(--primary-color); box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.1); }
        .selected-tags { display: flex; flex-wrap: wrap; gap: 6px; width: 100%; align-items: center; padding: 2px; }
        .tag {
            background: #eef2ff; color: var(--primary-color);
            font-size: 0.85rem; padding: 3px 10px; border-radius: 99px;
            display: flex; align-items: center; gap: 6px; font-weight: 600;
            border: 1px solid #c7d2fe;
        }
        .tag button { background: none; border: none; cursor: pointer; font-size: 1rem; color: inherit; padding: 0; line-height: 1; display: flex; opacity: 0.6; }
        .tag button:hover { opacity: 1; }
        
        .dropdown-list {
            position: absolute; top: calc(100% + 6px); left: 0; right: 0;
            background: white; border: 1px solid var(--border-color); border-radius: 8px;
            max-height: 320px; overflow-y: auto; box-shadow: var(--shadow-md); z-index: 100;
            padding: 6px 0;
        }
        .dropdown-item { padding: 10px 16px; font-size: 0.9rem; cursor: pointer; color: var(--text-color); transition: background 0.1s; }
        .dropdown-item:hover { background: #f8fafc; color: var(--primary-color); }
        .dropdown-item.add-custom { color: var(--primary-color); font-weight: 600; display: flex; align-items: center; gap: 8px; background: #f5f3ff;}

        /* RESULTS TABLES */
        .empty-state { margin-top: 10vh; text-align: center; color: var(--text-secondary); display: flex; flex-direction: column; align-items: center; gap: 1rem; }
        .empty-state svg { width: 64px; height: 64px; opacity: 0.15; color: var(--primary-color); }
        
        .zip-accordion-group {
            background: white;
            border: 1px solid var(--border-color);
            border-radius: 12px;
            overflow: hidden;
            margin-bottom: 1rem;
            box-shadow: var(--shadow-sm);
            transition: box-shadow 0.2s;
        }
        .zip-accordion-group:hover { box-shadow: var(--shadow-md); }

        .results-table { width: 100%; border-collapse: collapse; }
        .results-table thead th {
            padding: 1rem 1.25rem; text-align: left; font-size: 0.75rem; text-transform: uppercase; color: var(--text-secondary); font-weight: 700; letter-spacing: 0.05em;
            background: #f8fafc; border-bottom: 1px solid var(--border-color);
        }
        .results-table td { padding: 1rem 1.25rem; font-size: 0.9rem; vertical-align: top; border-bottom: 1px solid #f1f5f9; color: var(--text-color); }
        .results-table tr:last-child td { border-bottom: none; }
        .results-table tr:hover { background: #f8fafc; }
        .results-table tr.selected-row { background: #eff6ff; }
        .results-table tr.selected-row td { border-bottom-color: #dbeafe; }
        
        .email-tags { display: flex; flex-wrap: wrap; gap: 4px; }
        .email-tag { background: #dcfce7; color: #166534; padding: 2px 8px; border-radius: 4px; font-size: 0.75rem; border: 1px solid #bbf7d0; font-weight: 500;}
        .link-icon { color: var(--primary-color); text-decoration: none; font-size: 0.85rem; font-weight: 500; display: inline-flex; align-items: center; gap: 4px; }
        .link-icon:hover { text-decoration: underline; }

        /* MODALS */
        .modal-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(15, 23, 42, 0.4); display: flex; align-items: center; justify-content: center; z-index: 1000; backdrop-filter: blur(4px); }
        .modal-content { background: white; padding: 2.5rem; border-radius: 16px; width: 90%; max-width: 500px; box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04); }
        .modal-content.modal-large { max-width: 800px; }
        .modal-content h3 { margin-top: 0; margin-bottom: 0.5rem; font-size: 1.5rem; color: var(--text-color); }
        .sub-text { color: var(--text-secondary); font-size: 0.9rem; margin-bottom: 1.5rem; display: block; }
        .modal-form { display: flex; flex-direction: column; gap: 1rem; }
        .modal-actions { margin-top: 2rem; display: flex; justify-content: flex-end; gap: 0.75rem; }
        .modal-button { padding: 10px 20px; border-radius: 8px; cursor: pointer; font-weight: 600; border: 1px solid #cbd5e1; background: white; transition: all 0.2s;}
        .modal-button:hover { background: #f8fafc; }
        .modal-button.primary { background: var(--primary-color); color: white; border: none; box-shadow: 0 4px 6px -1px rgba(79, 70, 229, 0.3); }
        .modal-button.primary:hover { background: var(--primary-hover); transform: translateY(-1px); }

        .loader { display: flex; justify-content: center; align-items: center; height: 100vh; color: var(--text-secondary); font-weight: 500; gap: 10px; }
        
        /* AUTH */
        .auth-container { display: flex; justify-content: center; align-items: center; height: 100vh; background: #f8fafc; }
        .auth-form-wrapper { width: 100%; max-width: 380px; padding: 2.5rem; border-radius: 16px; background: white; box-shadow: var(--shadow-md); border: 1px solid var(--border-color); }
        .auth-form-wrapper h2 { text-align: center; margin-top: 0; margin-bottom: 2rem; color: var(--text-color); }
        .auth-form input { width: 100%; padding: 12px; margin-bottom: 12px; border-radius: 8px; border: 1px solid #cbd5e1; box-sizing: border-box; transition: border 0.2s; }
        .auth-form input:focus { border-color: var(--primary-color); outline: none; }
        .auth-form button { width: 100%; padding: 12px; background: var(--primary-color); color: white; border: none; border-radius: 8px; cursor: pointer; font-weight: 600; margin-top: 1rem; transition: background 0.2s; }
        .auth-form button:hover { background: var(--primary-hover); }
        .auth-toggle { text-align: center; margin-top: 1.5rem; font-size: 0.9rem; }
        .auth-toggle button { background: none; border: none; color: var(--primary-color); cursor: pointer; font-weight: 500; }
        .auth-toggle button:hover { text-decoration: underline; }
    </style>
<script type="importmap">
{
  "imports": {
    "react": "https://aistudiocdn.com/react@^19.2.0",
    "react-dom/": "https://aistudiocdn.com/react-dom@^19.2.0/",
    "react/": "https://aistudiocdn.com/react@^19.2.0/",
    "@google/genai": "https://aistudiocdn.com/@google/genai@^1.28.0",
    "firebase/app": "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js",
    "firebase/auth": "https://www.gstatic.com/firebasejs/10.12.2/firebase-auth.js",
    "firebase/storage": "https://www.gstatic.com/firebasejs/10.12.2/firebase-storage.js",
    "firebase/": "https://aistudiocdn.com/firebase@^12.6.0/"
  }
}
</script>
</head>
<body>
    <div id="root"></div>
    <script type="module" src="index.tsx"></script>
</body>
</html>


import React, { useState, useEffect, useRef, useMemo, useCallback } from 'react';
import { createRoot } from 'react-dom/client';
import { GoogleGenAI } from '@google/genai';
import { initializeApp } from "Supabase/app";
import {
    getAuth,
    onAuthStateChanged,
    signOut,
    User
} from "Supabase/auth";

// --- Supabase CONFIGURATION --- //
const SupabaseConfig = {
    apiKey: ",
    authDomain: ",
    projectId: "",
    storageBucket: ",
    messagingSenderId: "
    appId: "",
    measurementId: "
};

const app = initializeApp(SupabaseConfig);
const auth = getAuth(app);

// --- CONSTANTS --- //
// Updated URL to point to the correct file in the new repo (us_cities_states_counties_zips.csv)
const DATA_URL = `https://raw.githubusercontent.com/FunnelFunction/0.0_Locations-simplemaps.com/main/us_cities_states_counties_zips.csv?t=${Date.now()}`;

// --- ICONS (Inline SVGs) --- //
const Icons = {
    CheckSquare: ({className}: {className?: string}) => <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><rect x="3" y="3" width="18" height="18" rx="2" ry="2" fill="currentColor" fillOpacity="0.2"></rect><polyline points="9 11 12 14 22 4"></polyline></svg>,
    Square: ({className}: {className?: string}) => <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect></svg>,
    MinusSquare: ({className}: {className?: string}) => <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><rect x="3" y="3" width="18" height="18" rx="2" ry="2" fill="currentColor" fillOpacity="0.2"></rect><line x1="8" y1="12" x2="16" y2="12"></line></svg>,
    MapPin: () => <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path><circle cx="12" cy="10" r="3"></circle></svg>,
    Folder: () => <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M22 19a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h5l2 3h9a2 2 0 0 1 2 2z"></path></svg>,
    Save: () => <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2z"></path><polyline points="17 21 17 13 7 13 7 21"></polyline><polyline points="7 3 7 8 15 8"></polyline></svg>,
    Trash: () => <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path></svg>,
    Loader: () => <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="animate-spin"><line x1="12" y1="2" x2="12" y2="6"></line><line x1="12" y1="18" x2="12" y2="22"></line><line x1="4.93" y1="4.93" x2="7.76" y2="7.76"></line><line x1="16.24" y1="16.24" x2="19.07" y2="19.07"></line><line x1="2" y1="12" x2="6" y2="12"></line><line x1="18" y1="12" x2="22" y2="12"></line><line x1="4.93" y1="19.07" x2="7.76" y2="16.24"></line><line x1="16.24" y1="7.76" x2="19.07" y2="4.93"></line></svg>,
    Plus: () => <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>,
    X: () => <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>,
    Briefcase: () => <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><rect x="2" y="7" width="20" height="14" rx="2" ry="2"></rect><path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"></path></svg>,
    ChevronRight: ({className}: {className?: string}) => <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><polyline points="9 18 15 12 9 6"></polyline></svg>,
    ChevronDown: ({className}: {className?: string}) => <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><polyline points="6 9 12 15 18 9"></polyline></svg>,
};

// --- INTERFACES --- //
interface BusinessResult {
    id: string;
    category: string;
    name: string;
    address: string;
    phone: string;
    website: string;
    emails: string[];
    selected: boolean;
    zipCode?: string;
    city?: string;
    state?: string;
}

interface SearchSnapshot {
    id: number;
    name: string;
    categories: string[];
    leafIds: string[]; // Store array of strings for storage
    resultsMap: {[key: string]: BusinessResult[]};
    timestamp: string;
}

// --- TREE COMPONENT --- //
const CheckboxIcon = ({ status, className }: { status: 'checked' | 'unchecked' | 'indeterminate', className?: string }) => {
    if (status === 'checked') return <Icons.CheckSquare className={className} />;
    if (status === 'indeterminate') return <Icons.MinusSquare className={className} />;
    return <Icons.Square className={className} />;
};

const TreeNode = React.memo(({ node, level, selectedLeafs, toggleSelection, expandedNodes, toggleExpand }: any) => {
    const isExpanded = expandedNodes.has(node.id);
    const hasChildren = node.children && node.children.length > 0;
    
    const status = useMemo(() => {
        if (!hasChildren) return selectedLeafs.has(node.id) ? 'checked' : 'unchecked';
        let foundSelected = false;
        let checkedCount = 0;
        // Optimization: For large trees, don't iterate everything if we don't have to
        const leafCount = node.leafIds.length;
        if (leafCount === 0) return 'unchecked';

        // Checking overlap is expensive for large arrays. 
        // Logic: if all leafIds are in selectedLeafs -> checked
        // if some -> indeterminate
        // if none -> unchecked
        
        let matchCount = 0;
        for (const leafId of node.leafIds) {
            if (selectedLeafs.has(leafId)) matchCount++;
        }
        
        if (matchCount === 0) return 'unchecked';
        if (matchCount === leafCount) return 'checked';
        return 'indeterminate';
    }, [selectedLeafs, node, hasChildren]);

    return (
        <div className="tree-node-wrapper">
            <div 
                className={`tree-row level-${level} ${isExpanded ? 'expanded' : ''} ${status !== 'unchecked' ? 'active-selection' : ''}`}
                onClick={(e) => {
                    // Row click logic: Expand if group, Select if leaf
                    if (hasChildren) {
                        e.stopPropagation();
                        toggleExpand(node.id);
                    } else {
                        // For Zips (leafs), row click toggles selection
                        toggleSelection(node, status);
                    }
                }}
            >
                {/* Chevron: Handles expansion independently */}
                <div 
                    className={`tree-chevron ${isExpanded ? 'rotated' : ''} ${!hasChildren ? 'hidden' : ''}`}
                    onClick={(e) => {
                        e.stopPropagation();
                        if (hasChildren) toggleExpand(node.id);
                    }}
                >
                    <Icons.ChevronRight className="chevron-icon" />
                </div>
                
                {/* Checkbox: Handles Selection */}
                <div 
                    className={`tree-checkbox ${status}`} 
                    onClick={(e) => { 
                        e.stopPropagation(); 
                        toggleSelection(node, status); 
                    }}
                >
                    <CheckboxIcon status={status} className="checkbox-svg" />
                </div>

                {/* Label */}
                <div className="tree-content" style={{display:'flex', alignItems:'center', flex:1, overflow:'hidden'}}>
                    <span className="tree-label">{node.label}</span>
                    {hasChildren && (
                        <span className="tree-count">
                            {node.leafIds.length.toLocaleString()}
                        </span>
                    )}
                </div>
            </div>
            
            {/* Render Children */}
            {hasChildren && isExpanded && (
                <div className="tree-children">
                    {node.children.map((child: any) => (
                        <TreeNode 
                            key={child.id} node={child} level={level + 1} 
                            selectedLeafs={selectedLeafs} toggleSelection={toggleSelection}
                            expandedNodes={expandedNodes} toggleExpand={toggleExpand}
                        />
                    ))}
                </div>
            )}
        </div>
    );
});

// --- APP COMPONENT --- //
const App = () => {
    // Auth State
    const [user, setUser] = useState<User | null>(null);
    const [authLoading, setAuthLoading] = useState(true);

    // Search Params
    const [categoriesList, setCategoriesList] = useState<string[]>([]);
    const [selectedCategories, setSelectedCategories] = useState<string[]>([]); // Multi-select
    const [categorySearchTerm, setCategorySearchTerm] = useState("");
    const [isCategoryDropdownOpen, setIsCategoryDropdownOpen] = useState(false);
    
    // Location Tree State
    const [treeData, setTreeData] = useState<any[]>([]);
    const [treeLoading, setTreeLoading] = useState(true);
    const [treeError, setTreeError] = useState("");
    const [selectedLeafs, setSelectedLeafs] = useState<Set<string>>(new Set());
    const [expandedNodes, setExpandedNodes] = useState<Set<string>>(new Set());
    const [reverseLookup, setReverseLookup] = useState<any>({});

    // Results State (Grouped by Category)
    const [resultsMap, setResultsMap] = useState<{[key: string]: BusinessResult[]}>({});
    const [expandedResults, setExpandedResults] = useState<Set<string>>(new Set());
    const [isSearching, setIsSearching] = useState(false);
    const [searchProgress, setSearchProgress] = useState("");

    // Accordion / Snapshots
    const [savedSnapshots, setSavedSnapshots] = useState<SearchSnapshot[]>([]);
    const [isAccordionOpen, setIsAccordionOpen] = useState(false);
    
    // Save Modal
    const [isSaveModalOpen, setIsSaveModalOpen] = useState(false);
    const [snapshotName, setSnapshotName] = useState("");

    // Email Modal
    const [isEmailModalOpen, setIsEmailModalOpen] = useState(false);
    const [emailSubject, setEmailSubject] = useState("Collaboration Opportunity");
    const [emailBody, setEmailBody] = useState("");

    const categoryInputRef = useRef<HTMLInputElement>(null);

    // --- INITIALIZATION --- //
    useEffect(() => {
        const unsubscribe = onAuthStateChanged(auth, (currentUser) => {
            setUser(currentUser);
            setAuthLoading(false);
        });

        fetch('./google-business-categories.json')
            .then(res => res.json())
            .then(data => {
                const cleanList = data.filter((d: string) => d.length > 1 && d !== "Business Categories");
                cleanList.sort(); // Ensure A-Z sort
                setCategoriesList(cleanList);
            })
            .catch(err => console.error("Failed to load categories", err));

        const loaded = localStorage.getItem('armstrong_snapshots');
        if (loaded) {
            try { setSavedSnapshots(JSON.parse(loaded)); } catch(e) {}
        }

        // FETCHING LOCATION DATA
        console.log("Fetching location data from:", DATA_URL);
        fetch(DATA_URL)
            .then(res => {
                if (!res.ok) throw new Error(`Network response was not ok: ${res.statusText}`);
                return res.text();
            })
            .then(rawText => {
                // Remove Byte Order Mark if present
                let text = rawText;
                if (text.charCodeAt(0) === 0xFEFF) {
                    text = text.slice(1);
                }

                const lines = text.split('\n');
                console.log(`Loaded CSV with ${lines.length} lines.`);

                const hierarchy: any = {};
                const lookup: any = {};
                
                // DETECT FORMAT
                const firstLine = lines[0].toLowerCase().trim();
                const isPipeDelimited = firstLine.includes('|');
                
                // Basic CSV Splitter (comma)
                const splitCSV = (line: string) => {
                    return line.split(/,(?=(?:(?:[^"]*"){2})*[^"]*$)/).map(v => v.replace(/^"|"$/g, '').trim());
                };

                lines.forEach((line, idx) => {
                    if (!line.trim()) return;
                    if (idx === 0 && (line.toLowerCase().startsWith('city') || line.toLowerCase().startsWith('zip'))) return; // Skip header
                    
                    let city, stateShort, stateFull, county, zipList: string[] = [];

                    if (isPipeDelimited) {
                        const parts = line.split('|');
                        if (parts.length < 2) return;
                        
                        city = parts[0]?.trim();
                        stateShort = parts[1]?.trim();
                        stateFull = parts[2]?.trim();
                        county = parts[3]?.trim();
                        
                        // Zips are in the 7th column (index 6), comma separated or space separated
                        const rawZips = parts[6]?.trim();
                        if (rawZips) {
                            // Extract all valid 5-digit sequences
                            const foundZips = rawZips.match(/\d{5}/g);
                            if (foundZips) zipList = foundZips;
                        }
                    } else {
                        // Fallback Legacy Format
                        const parts = splitCSV(line);
                        // Standard SimpleMaps free database format:
                        // zip,lat,lng,city,state_id,state_name,zcta,parent_zcta,population,density,county_fips,county_name,...
                        if (parts[0] && parts[3] && parts[4]) {
                            zipList = [parts[0]];
                            city = parts[3];
                            stateShort = parts[4];
                            stateFull = parts[5];
                            county = parts[11] || 'Unknown';
                        }
                    }
                    
                    if (!stateShort || !city || zipList.length === 0) return;

                    const fullStateName = stateFull || stateShort;
                    
                    // Build Hierarchy
                    if (!hierarchy[stateShort]) hierarchy[stateShort] = { id: `state-${stateShort}`, label: fullStateName, type: 'state', counties: {}, leafIds: [] };
                    if (!hierarchy[stateShort].counties[county]) hierarchy[stateShort].counties[county] = { id: `county-${stateShort}-${county}`, label: county, type: 'county', cities: {}, leafIds: [] };
                    if (!hierarchy[stateShort].counties[county].cities[city]) hierarchy[stateShort].counties[county].cities[city] = { id: `city-${stateShort}-${county}-${city}`, label: city, type: 'city', zips: [], leafIds: [] };
                    
                    const cityNode = hierarchy[stateShort].counties[county].cities[city];
                    
                    // Add all zips found for this city
                    zipList.forEach(zip => {
                        const zipId = `zip-${zip}`;
                        if (!/^\d{5}$/.test(zip)) return;

                        // Check if zip already added to this city (avoid duplicates)
                        if (!cityNode.zips.some((z:any) => z.id === zipId)) {
                            const zipNode = { id: zipId, label: zip, type: 'zip', leafIds: [zipId] };
                            cityNode.zips.push(zipNode);
                            
                            // Propagate Leaf IDs up the chain
                            cityNode.leafIds.push(zipId);
                            hierarchy[stateShort].counties[county].leafIds.push(zipId);
                            hierarchy[stateShort].leafIds.push(zipId);
                            
                            lookup[zipId] = { state: fullStateName, county, city, zip };
                        }
                    });
                });

                // IMPORTANT: Set reverse lookup state so search works!
                setReverseLookup(lookup);
                console.log(`Processed ${Object.keys(lookup).length} unique zip codes.`);

                const tree = Object.values(hierarchy).sort((a:any, b:any) => a.label.localeCompare(b.label)).map((state:any) => ({
                    ...state,
                    children: Object.values(state.counties).sort((a:any, b:any) => a.label.localeCompare(b.label)).map((county:any) => ({
                        ...county,
                        children: Object.values(county.cities).sort((a:any, b:any) => a.label.localeCompare(b.label)).map((city:any) => ({
                            ...city,
                            children: city.zips.sort((a:any, b:any) => a.label.localeCompare(b.label))
                        }))
                    }))
                }));

                if (tree.length === 0) {
                    setTreeError("No location data found in file.");
                } else {
                    setTreeData(tree);
                }
                setTreeLoading(false);
            })
            .catch(err => {
                console.error("Failed to load map data", err);
                setTreeError("Failed to load locations. Please check connection.");
                setTreeLoading(false);
            });

        return () => unsubscribe();
    }, []);

    // --- LOCATION HANDLERS --- //
    const toggleExpand = useCallback((nodeId: string) => {
        setExpandedNodes(prev => {
            const next = new Set(prev);
            if (next.has(nodeId)) next.delete(nodeId);
            else next.add(nodeId);
            return next;
        });
    }, []);

    const toggleSelection = useCallback((node: any, currentStatus: string) => {
        setSelectedLeafs(prev => {
            const next = new Set(prev);
            if (currentStatus === 'checked') {
                node.leafIds.forEach((id: string) => next.delete(id));
            } else {
                node.leafIds.forEach((id: string) => next.add(id));
            }
            return next;
        });
    }, []);

    // --- CATEGORY HANDLERS --- //
    const addCategory = (cat: string) => {
        if (!selectedCategories.includes(cat)) {
            setSelectedCategories([...selectedCategories, cat]);
        }
        setCategorySearchTerm("");
        setIsCategoryDropdownOpen(false);
    };

    const removeCategory = (cat: string) => {
        setSelectedCategories(prev => prev.filter(c => c !== cat));
    };

    const createCustomCategory = () => {
        if (categorySearchTerm && !categoriesList.includes(categorySearchTerm)) {
            const newCat = categorySearchTerm;
            setCategoriesList(prev => [...prev, newCat]);
            addCategory(newCat);
        }
    };

    // --- SNAPSHOTS LOGIC --- //
    const initiateSave = () => {
        setSnapshotName("");
        setIsSaveModalOpen(true);
    };

    const confirmSaveSnapshot = () => {
        if (Object.keys(resultsMap).length === 0) return;
        
        const totalBusinesses = Object.values(resultsMap).flat().length;
        const name = snapshotName || `Search ${new Date().toLocaleDateString()}`;

        const snapshot: SearchSnapshot = {
            id: Date.now(),
            name: name,
            categories: selectedCategories,
            leafIds: Array.from(selectedLeafs),
            resultsMap: resultsMap,
            timestamp: new Date().toLocaleString()
        };

        const updated = [snapshot, ...savedSnapshots];
        setSavedSnapshots(updated);
        localStorage.setItem('armstrong_snapshots', JSON.stringify(updated));
        
        setIsSaveModalOpen(false);
        setIsAccordionOpen(true);
        alert("Snapshot saved successfully.");
    };

    const loadSnapshot = (snapshot: SearchSnapshot) => {
        if (!confirm(`Load snapshot "${snapshot.name}"? This will replace current results.`)) return;
        
        // Restore Categories
        setSelectedCategories(snapshot.categories || []); // Handle legacy single-cat snapshots
        
        // Restore Locations (Convert array back to Set)
        setSelectedLeafs(new Set(snapshot.leafIds));
        
        // Restore Results
        setResultsMap(snapshot.resultsMap);
        setExpandedResults(new Set(Object.keys(snapshot.resultsMap)));
        
        setIsAccordionOpen(false);
    };

    const deleteSnapshot = (id: number, e: React.MouseEvent) => {
        e.stopPropagation();
        const updated = savedSnapshots.filter(s => s.id !== id);
        setSavedSnapshots(updated);
        localStorage.setItem('armstrong_snapshots', JSON.stringify(updated));
    };

    // --- SEARCH LOGIC --- //
    const handleSearch = async () => {
        if (selectedCategories.length === 0) {
            alert("Please select at least one business category.");
            return;
        }
        if (selectedLeafs.size === 0) {
            alert("Please select at least one location from the sidebar.");
            return;
        }

        setIsSearching(true);
        setResultsMap({}); 
        setExpandedResults(new Set());

        const zipIds = Array.from(selectedLeafs);
        // Limit for demo performance
        const MAX_ZIPS = 100;
        const processingZips = zipIds.length > MAX_ZIPS ? zipIds.slice(0, MAX_ZIPS) : zipIds;
        
        console.log(`Starting Search. Total Selected: ${zipIds.length}, Processing: ${processingZips.length}`);
        console.log(`Categories:`, selectedCategories);
        console.log(`Reverse Lookup Size: ${Object.keys(reverseLookup).length}`);

        if (zipIds.length > MAX_ZIPS) alert(`For performance, searching first ${MAX_ZIPS} zip codes out of ${zipIds.length} selected.`);

        const ai = new GoogleGenAI({ apiKey: process.env.API_KEY });

        for (const zipId of processingZips) {
            const location = reverseLookup[zipId];
            if (!location) {
                console.warn(`No location found for ZipID: ${zipId}`);
                continue;
            }
            const { city, state, zip } = location;
            console.log(`Processing Zip: ${zip} (${city}, ${state})`);

            // Loop through ALL selected categories for this zip
            for (const category of selectedCategories) {
                setSearchProgress(`Searching for ${category} in ${city}, ${state} (${zip})...`);
                
                try {
                    const prompt = `Find 3 real ${category} businesses in Zip Code ${zip} (${city}, ${state}). 
                    Return a valid JSON array. Each object: 'name', 'address', 'phone', 'website'. 
                    Strict JSON only.`;
                    
                    console.log(`Sending Prompt for ${category}:`, prompt);

                    const response = await ai.models.generateContent({
                        model: "gemini-2.5-flash",
                        contents: prompt,
                        config: { tools: [{googleSearch: {}}] }
                    });

                    const text = response.text || "";
                    console.log(`Received Response for ${category}:`, text);

                    let cleanJson = text.replace(/```json|```/g, '').trim();
                    const start = cleanJson.indexOf('[');
                    const end = cleanJson.lastIndexOf(']');
                    if (start !== -1 && end !== -1) cleanJson = cleanJson.substring(start, end + 1);

                    let data = [];
                    try { data = JSON.parse(cleanJson); } catch(e) {
                         console.error("JSON Parsing Failed:", e);
                    }

                    if (Array.isArray(data) && data.length > 0) {
                        const mapped: BusinessResult[] = data.map((item: any, idx: number) => ({
                            id: `${zip}-${category}-${idx}`,
                            category: category,
                            name: item.name || "Unknown",
                            address: (item.address && typeof item.address === 'object') ? Object.values(item.address).join(', ') : item.address || "No address",
                            phone: item.phone || "No phone",
                            website: item.website || "",
                            emails: [],
                            selected: false,
                            zipCode: zip,
                            city: city,
                            state: state
                        }));
                        
                        // Group by CATEGORY
                        setResultsMap(prev => ({
                            ...prev,
                            [category]: [...(prev[category] || []), ...mapped]
                        }));
                        setExpandedResults(prev => new Set(prev).add(category));
                        
                        // Background scrape
                        scrapeContacts(mapped, city, state);
                    } else {
                        console.log("No data or invalid format returned.");
                    }
                } catch (err) {
                    console.error(`Failed search for ${category} in ${zip}`, err);
                }
            }
        }

        setSearchProgress("");
        setIsSearching(false);
        console.log("Search Complete.");
    };

    const scrapeContacts = async (businesses: BusinessResult[], city: string, state: string) => {
        const ai = new GoogleGenAI({ apiKey: process.env.API_KEY });
        for (const biz of businesses) {
            if (!biz.website) continue;
            try {
                const prompt = `Find contact email for ${biz.name} in ${city}. Return JSON array of strings e.g. ["info@x.com"].`;
                const res = await ai.models.generateContent({
                    model: "gemini-2.5-flash",
                    contents: prompt,
                    config: { tools: [{googleSearch: {}}] }
                });
                
                let clean = (res.text || "[]").replace(/```json|```/g, '').trim();
                const s = clean.indexOf('['); const e = clean.lastIndexOf(']');
                if (s!==-1 && e!==-1) clean = clean.substring(s, e+1);
                
                let emails = [];
                try { emails = JSON.parse(clean); } catch(ex){}
                
                if (Array.isArray(emails) && emails.length > 0) {
                    setResultsMap(prev => {
                        // Locate the category group for this business
                        const categoryList = prev[biz.category] || [];
                        const newList = categoryList.map(b => b.id === biz.id ? { ...b, emails } : b);
                        return { ...prev, [biz.category]: newList };
                    });
                }
            } catch(err) {}
        }
    };

    // --- UI HELPERS --- //
    const filteredCategories = useMemo(() => {
        if (!categorySearchTerm) return categoriesList; // Return ALL for browsing (A-Z)
        return categoriesList.filter(c => c.toLowerCase().includes(categorySearchTerm.toLowerCase()));
    }, [categoriesList, categorySearchTerm]);

    const toggleGroupAccordion = (key: string) => {
        setExpandedResults(prev => {
            const next = new Set(prev);
            if (next.has(key)) next.delete(key); else next.add(key);
            return next;
        });
    };

    const toggleSelectBusiness = (categoryKey: string, id: string) => {
        setResultsMap(prev => {
            const list = prev[categoryKey] || [];
            const newList = list.map(b => b.id === id ? { ...b, selected: !b.selected } : b);
            return { ...prev, [categoryKey]: newList };
        });
    };

    const allResults = Object.values(resultsMap).flat();
    const selectedResults = allResults.filter(r => r.selected);

    if (authLoading) return <div className="loader"><Icons.Loader /></div>;
    if (!user) return <AuthScreen />;

    return (
        <div className="main-layout">
            {/* --- TOP BAR --- */}
            <header className="control-bar glass-pane">
                <div className="control-group logo-group">
                    <button 
                        className={`icon-button ${isAccordionOpen ? 'active' : ''}`}
                        onClick={() => setIsAccordionOpen(!isAccordionOpen)}
                        title="Saved Snapshots"
                    >
                        <Icons.Folder />
                    </button>
                    <div className="app-logo">Armstrong.</div>
                </div>

                <div className="inputs-group">
                    {/* Multi-Select Category Input */}
                    <div className="custom-multiselect" style={{width: '450px'}} onClick={() => categoryInputRef.current?.focus()}>
                        <div className="selected-tags">
                            {selectedCategories.map(cat => (
                                <span key={cat} className="tag">
                                    {cat} <button onClick={(e) => { e.stopPropagation(); removeCategory(cat); }}><Icons.X /></button>
                                </span>
                            ))}
                            <input 
                                ref={categoryInputRef}
                                type="text"
                                className="no-bg-input"
                                placeholder={selectedCategories.length === 0 ? "Search Business Categories..." : ""}
                                value={categorySearchTerm}
                                onChange={(e) => {
                                    setCategorySearchTerm(e.target.value);
                                    setIsCategoryDropdownOpen(true);
                                }}
                                onFocus={() => setIsCategoryDropdownOpen(true)}
                                onKeyDown={(e) => {
                                    if(e.key === 'Backspace' && categorySearchTerm === '' && selectedCategories.length > 0) {
                                        removeCategory(selectedCategories[selectedCategories.length-1]);
                                    }
                                }}
                            />
                        </div>
                        {isCategoryDropdownOpen && (
                            <div className="dropdown-list">
                                {!categorySearchTerm && (
                                    <div className="p-3 text-xs text-slate-400 font-medium tracking-wide uppercase">All Categories (A-Z)</div>
                                )}
                                {filteredCategories.length === 0 && categorySearchTerm && (
                                    <div className="dropdown-item add-custom" onMouseDown={(e) => { e.preventDefault(); createCustomCategory(); }}>
                                        <Icons.Plus /> Add "{categorySearchTerm}"
                                    </div>
                                )}
                                {filteredCategories.map((cat, idx) => (
                                    <div key={idx} className="dropdown-item" onMouseDown={(e) => { e.preventDefault(); addCategory(cat); }}>
                                        {cat}
                                    </div>
                                ))}
                            </div>
                        )}
                    </div>

                    <button className="chrome-button primary" onClick={handleSearch} disabled={isSearching || selectedCategories.length === 0}>
                        {isSearching ? <span style={{display:'flex', gap:'4px'}}><Icons.Loader /> Searching...</span> : "Search"}
                    </button>

                    {Object.keys(resultsMap).length > 0 && !isSearching && (
                        <button 
                            className="chrome-button" 
                            onClick={initiateSave}
                            style={{display:'flex', alignItems:'center', gap:'5px', color:'var(--primary-color)'}}
                        >
                            <Icons.Save /> Save
                        </button>
                    )}
                </div>

                <div className="control-group actions-group">
                     <button className="chrome-button send-btn" onClick={() => { if(selectedResults.length>0) setIsEmailModalOpen(true); }}>
                        Email ({selectedResults.length})
                     </button>
                     <div className="user-avatar" title={user.email || ""}>
                        {user.email ? user.email[0].toUpperCase() : "U"}
                     </div>
                     <button className="sign-out-icon" onClick={() => signOut(auth)}>
                        <svg width="20" height="20" fill="none" stroke="currentColor" strokeWidth="2" viewBox="0 0 24 24"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"></path><polyline points="16 17 21 12 16 7"></polyline><line x1="21" y1="12" x2="9" y2="12"></line></svg>
                     </button>
                </div>
            </header>

            {/* --- SAVED SNAPSHOTS DROPDOWN --- */}
            <div className={`accordion-panel ${isAccordionOpen ? 'open' : ''}`}>
                <div className="accordion-content">
                    <h3 className="text-xl font-bold text-slate-800 mb-6">Saved Search Snapshots</h3>
                    {savedSnapshots.length === 0 ? (
                        <div className="text-center p-8 text-slate-400 border-2 border-dashed border-slate-200 rounded-xl">No saved results yet. Save a search to see it here.</div>
                    ) : (
                        <div className="saved-searches-grid">
                            {savedSnapshots.map(snap => (
                                <div key={snap.id} className="snapshot-card" onClick={() => loadSnapshot(snap)}>
                                    <button className="delete-save-btn" onClick={(e) => deleteSnapshot(snap.id, e)}><Icons.Trash /></button>
                                    <div className="save-cat truncate">{snap.name}</div>
                                    <div className="snapshot-meta">
                                        <span>{snap.timestamp.split(',')[0]}</span>
                                    </div>
                                    <div className="snapshot-badges">
                                        <span className="badge location">{snap.leafIds.length} Zips</span>
                                        <span className="badge business">{Object.values(snap.resultsMap).flat().length} Biz</span>
                                    </div>
                                    <div className="text-[10px] text-slate-400 mt-2 truncate font-medium">
                                        {snap.categories ? snap.categories.join(', ') : 'Single Category'}
                                    </div>
                                </div>
                            ))}
                        </div>
                    )}
                </div>
            </div>

            {/* --- MAIN BODY: SIDEBAR + RESULTS --- */}
            <div className="app-body">
                
                {/* LEFT SIDEBAR: Geographic Master List */}
                <aside className="location-sidebar">
                    <div className="sidebar-header">
                        <div className="sidebar-title">
                            <Icons.MapPin />
                            <span>Locations</span>
                        </div>
                        <div className="sidebar-badge">
                            {selectedLeafs.size} Selected
                        </div>
                    </div>
                    
                    <div className="tree-container">
                        {treeLoading ? (
                            <div className="text-center p-8 text-slate-400 text-sm flex flex-col items-center gap-2">
                                <Icons.Loader /> 
                                <span>Loading Map Data...</span>
                            </div>
                        ) : treeError ? (
                            <div className="text-center p-8 text-red-400 text-sm flex flex-col items-center gap-2">
                                <span>{treeError}</span>
                                <button className="chrome-button" onClick={() => window.location.reload()}>Retry</button>
                            </div>
                        ) : (
                            treeData.map(node => (
                                <TreeNode 
                                    key={node.id} 
                                    node={node} 
                                    level={0} 
                                    selectedLeafs={selectedLeafs} 
                                    toggleSelection={toggleSelection} 
                                    expandedNodes={expandedNodes} 
                                    toggleExpand={toggleExpand} 
                                />
                            ))
                        )}
                    </div>
                </aside>

                {/* RIGHT CONTENT: RESULTS */}
                <main className="results-content">
                    {isSearching && <div className="p-4 text-center text-indigo-600 font-medium sticky top-0 bg-white/90 backdrop-blur z-20 shadow-sm border-b border-indigo-100 rounded-lg mb-4">{searchProgress}</div>}
                    
                    {Object.keys(resultsMap).length === 0 && !isSearching ? (
                        <div className="empty-state">
                            <Icons.MapPin />
                            <div>
                                <p className="text-xl font-bold text-slate-700">Ready to Outreach</p>
                                <p className="text-sm text-slate-500 mt-2 max-w-md mx-auto leading-relaxed">
                                    Select target locations from the sidebar,<br/>
                                    add business categories above,<br/>
                                    and click Search to find leads.
                                </p>
                            </div>
                        </div>
                    ) : (
                        <div className="results-container">
                            {Object.entries(resultsMap).map(([categoryName, businesses]) => {
                                const isOpen = expandedResults.has(categoryName);
                                const selectedCount = businesses.filter(b => b.selected).length;

                                return (
                                    <div key={categoryName} className="zip-accordion-group">
                                        <div 
                                            onClick={() => toggleGroupAccordion(categoryName)}
                                            style={{
                                                padding:'1rem 1.5rem', 
                                                background: isOpen ? '#f8fafc' : 'white', 
                                                cursor:'pointer', 
                                                display:'flex', 
                                                justifyContent:'space-between',
                                                alignItems:'center',
                                                borderBottom: isOpen ? '1px solid #e2e8f0' : 'none'
                                            }}
                                        >
                                            <div style={{display:'flex', alignItems:'center', gap:'12px'}}>
                                                <span style={{color: '#94a3b8', display:'flex', transform: isOpen ? 'rotate(90deg)' : 'none', transition: 'transform 0.2s'}}>
                                                    <Icons.ChevronRight />
                                                </span>
                                                <div>
                                                    <span className="font-bold text-slate-800" style={{fontSize:'1.05rem'}}>{categoryName}</span>
                                                    <span className="ml-3 text-slate-500 font-bold text-xs bg-slate-100 px-2 py-1 rounded-md border border-slate-200">
                                                        {businesses.length} Found
                                                    </span>
                                                </div>
                                            </div>
                                            <div className="text-sm text-slate-500">
                                                {selectedCount > 0 && <span className="text-indigo-600 font-bold mr-3 bg-indigo-50 px-2 py-1 rounded-md">{selectedCount} Selected</span>}
                                            </div>
                                        </div>

                                        {isOpen && (
                                            <div className="zip-accordion-body">
                                                <table className="results-table">
                                                    <thead>
                                                        <tr>
                                                            <th style={{width:'40px'}}></th>
                                                            <th>Business Name</th>
                                                            <th>Location</th>
                                                            <th>Details</th>
                                                            <th>Contact</th>
                                                        </tr>
                                                    </thead>
                                                    <tbody>
                                                        {businesses.map(biz => (
                                                            <tr key={biz.id} className={biz.selected ? "selected-row" : ""}>
                                                                <td>
                                                                    <div className="flex justify-center">
                                                                        <input type="checkbox" checked={biz.selected} onChange={() => toggleSelectBusiness(categoryName, biz.id)} style={{width:'16px', height:'16px', cursor:'pointer'}} />
                                                                    </div>
                                                                </td>
                                                                <td className="font-bold text-slate-800">{biz.name}</td>
                                                                <td>
                                                                    <div className="text-xs font-semibold text-slate-700">{biz.city}, {biz.state}</div>
                                                                    <div className="text-[10px] text-slate-400 font-mono mt-0.5">{biz.zipCode}</div>
                                                                </td>
                                                                <td>
                                                                    <div className="text-xs text-slate-600 mb-1">{biz.address}</div>
                                                                    {biz.website && <a href={biz.website} target="_blank" className="link-icon">Website <Icons.ChevronRight className="w-3 h-3"/></a>}
                                                                </td>
                                                                <td>
                                                                    <div className="text-xs font-medium text-slate-700 mb-1">{biz.phone}</div>
                                                                    <div className="email-tags">
                                                                        {biz.emails.length > 0 ? biz.emails.map((e,i) => <span key={i} className="email-tag">{e}</span>) : <span className="text-xs text-slate-300">-</span>}
                                                                    </div>
                                                                </td>
                                                            </tr>
                                                        ))}
                                                    </tbody>
                                                </table>
                                            </div>
                                        )}
                                    </div>
                                );
                            })}
                        </div>
                    )}
                </main>
            </div>

            {/* --- MODALS --- */}
            {isSaveModalOpen && (
                <div className="modal-overlay">
                    <div className="modal-content">
                        <h3>Save Snapshot</h3>
                        <p className="sub-text">Give this search result a memorable name.</p>
                        <input 
                            className="input-solid w-full p-3 mb-4 text-lg" 
                            autoFocus
                            placeholder="e.g. Austin Gyms & Spas" 
                            value={snapshotName} 
                            onChange={e => setSnapshotName(e.target.value)}
                            onKeyDown={e => e.key === 'Enter' && confirmSaveSnapshot()}
                        />
                        <div className="modal-actions">
                            <button className="modal-button" onClick={() => setIsSaveModalOpen(false)}>Cancel</button>
                            <button className="modal-button primary" onClick={confirmSaveSnapshot}>Save Search</button>
                        </div>
                    </div>
                </div>
            )}

            {isEmailModalOpen && (
                <div className="modal-overlay">
                    <div className="modal-content modal-large">
                        <h3>Compose Bulk Email</h3>
                        <p className="sub-text">Sending to {selectedResults.length} recipients.</p>
                        <div className="modal-form">
                            <label className="text-sm font-bold text-slate-700">Subject</label>
                            <input className="input-solid p-3" type="text" value={emailSubject} onChange={(e) => setEmailSubject(e.target.value)} />
                            <label className="text-sm font-bold text-slate-700">Body</label>
                            <textarea className="input-solid p-3 font-sans" rows={10} value={emailBody} onChange={(e) => setEmailBody(e.target.value)}></textarea>
                        </div>
                        <div className="modal-actions">
                            <button className="modal-button" onClick={() => setIsEmailModalOpen(false)} style={{color:'black'}}>Cancel</button>
                            <button className="modal-button primary" onClick={() => setIsEmailModalOpen(false)}>Send Emails</button>
                        </div>
                    </div>
                </div>
            )}
        </div>
    );
};

const AuthScreen = () => {
    const [isLogin, setIsLogin] = useState(true);
    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");
    const handleAuth = async (e: React.FormEvent) => {
        e.preventDefault();
        try {
            if (isLogin) await import("Supabase/auth").then(({signInWithEmailAndPassword}) => signInWithEmailAndPassword(auth, email, password));
            else await import("Supabase/auth").then(({createUserWithEmailAndPassword}) => createUserWithEmailAndPassword(auth, email, password));
        } catch (err: any) { alert(err.message); }
    };
    return (
        <div className="auth-container">
            <div className="auth-form-wrapper">
                <h2>{isLogin ? "Sign In" : "Create Account"}</h2>
                <form className="auth-form" onSubmit={handleAuth}>
                    <input type="email" placeholder="Email" value={email} onChange={e => setEmail(e.target.value)} required />
                    <input type="password" placeholder="Password" value={password} onChange={e => setPassword(e.target.value)} required />
                    <button type="submit">{isLogin ? "Sign In" : "Sign Up"}</button>
                </form>
                <div className="auth-toggle"><button onClick={() => setIsLogin(!isLogin)}>{isLogin ? "Need account? Create one" : "Already have an account? Sign In"}</button></div>
            </div>
        </div>
    );
};

const root = createRoot(document.getElementById('root')!);
root.render(<App />);



