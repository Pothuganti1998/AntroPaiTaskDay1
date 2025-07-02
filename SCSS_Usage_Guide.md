# SCSS Media Queries Usage Guide for Lead Management

## Overview
This guide explains how to implement the responsive SCSS styles for your Lead Management React component.

## File Structure
```
src/
├── components/
│   └── LeadManagement.jsx
├── styles/
│   └── LeadManagement.scss
└── App.jsx
```

## Implementation Steps

### 1. Install SCSS Support
If you haven't already, install SCSS support:

```bash
npm install sass
# or
yarn add sass
```

### 2. Import SCSS in Your Component
Update your React component to import the SCSS file:

```jsx
// LeadManagement.jsx
import React, { useState, useMemo, useRef, useEffect } from 'react';
import { Search, Filter, Download, Plus, Phone, Mail, Calendar, MapPin, ChevronRight, X, MoreVertical, Eye, FileText, Edit, Trash2 } from 'lucide-react';
import './LeadManagement.scss'; // Add this import

const LeadManagement = () => {
    // Your existing component code...
    
    return (
        <div className="lead-management"> {/* Add this wrapper class */}
            <div className="container">
                {/* Header */}
                <div className="header">
                    <h1>Lead Management</h1>
                    <button className="add-button" onClick={() => setShowAddModal(true)}>
                        <Plus size={20} />
                        Add New Lead
                    </button>
                </div>

                {/* Controls */}
                <div className="controls">
                    <div className="controls-row">
                        <div className="search-container">
                            <Search size={20} className="text-gray-400" />
                            <input
                                type="text"
                                placeholder="Search leads..."
                                value={searchTerm}
                                onChange={(e) => setSearchTerm(e.target.value)}
                            />
                        </div>

                        <div className="control-buttons">
                            <div className="view-control">
                                <span>View</span>
                                <select 
                                    value={viewMode} 
                                    onChange={(e) => setViewMode(Number(e.target.value))}
                                >
                                    <option value={5}>5</option>
                                    <option value={9}>9</option>
                                    <option value={15}>15</option>
                                    <option value={25}>25</option>
                                </select>
                            </div>

                            <button
                                className={`${showFilters ? 'active' : ''}`}
                                onClick={() => setShowFilters(!showFilters)}
                            >
                                <Filter size={16} />
                                Filters
                            </button>

                            <button onClick={() => setShowAdvancedFilters(true)}>
                                <Filter size={16} />
                                Advanced Filters
                            </button>

                            <button onClick={exportData}>
                                <Download size={16} />
                                Export
                            </button>
                        </div>
                    </div>

                    {/* Basic Filters */}
                    {showFilters && (
                        <div className="basic-filters">
                            <div className="filter-group">
                                <label>Status:</label>
                                <select 
                                    value={filterStatus} 
                                    onChange={(e) => setFilterStatus(e.target.value)}
                                >
                                    <option value="all">All Status</option>
                                    <option value="New">New</option>
                                    <option value="Contacted">Contacted</option>
                                    <option value="Quoted">Quoted</option>
                                    <option value="Follow-Up">Follow-Up</option>
                                    <option value="Converted">Converted</option>
                                    <option value="Rejected">Rejected</option>
                                </select>
                            </div>

                            <div className="filter-group">
                                <label>Source:</label>
                                <select 
                                    value={filterSource} 
                                    onChange={(e) => setFilterSource(e.target.value)}
                                >
                                    <option value="all">All Sources</option>
                                    <option value="Website">Website</option>
                                    <option value="Instagram">Instagram</option>
                                    <option value="Referral">Referral</option>
                                </select>
                            </div>
                        </div>
                    )}
                </div>

                {/* Table */}
                <div className="table-container">
                    <div className="table-wrapper">
                        <table>
                            {/* Your existing table content */}
                        </table>
                    </div>
                </div>

                {/* Modals with proper classes */}
                {showViewModal && currentLead && (
                    <div className="modal-overlay">
                        <div className="modal large">
                            <div className="modal-header">
                                <h2>Lead Details</h2>
                                <button onClick={() => setShowViewModal(false)}>
                                    <X size={20} />
                                </button>
                            </div>
                            <div className="modal-content">
                                <div className="form-grid">
                                    {/* Your modal content */}
                                </div>
                            </div>
                        </div>
                    </div>
                )}

                {/* Add Lead Modal */}
                {showAddModal && (
                    <div className="modal-overlay">
                        <div className="modal extra-large">
                            <div className="modal-header">
                                <h2>Add New Lead</h2>
                                <button onClick={() => setShowAddModal(false)}>
                                    <X size={20} />
                                </button>
                            </div>
                            <div className="modal-content">
                                <div className="form-grid">
                                    <div className="form-group">
                                        <label>Client Name *</label>
                                        <input
                                            type="text"
                                            value={newLead.clientInfo.name}
                                            onChange={(e) => setNewLead(prev => ({
                                                ...prev,
                                                clientInfo: { ...prev.clientInfo, name: e.target.value }
                                            }))}
                                            required
                                        />
                                    </div>
                                    {/* Other form fields */}
                                </div>
                            </div>
                            <div className="modal-footer">
                                <button 
                                    className="secondary"
                                    onClick={() => setShowAddModal(false)}
                                >
                                    Cancel
                                </button>
                                <button
                                    className="primary"
                                    onClick={handleAddLead}
                                    disabled={!newLead.clientInfo.name || !newLead.clientInfo.phone || !newLead.city || !newLead.serviceType}
                                >
                                    Add Lead
                                </button>
                            </div>
                        </div>
                    </div>
                )}

                {/* Advanced Filters Modal */}
                {showAdvancedFilters && (
                    <div className="modal-overlay">
                        <div className="modal extra-large advanced-filters">
                            <div className="modal-header">
                                <h2>Advanced Filters</h2>
                                <button onClick={() => setShowAdvancedFilters(false)}>
                                    <X size={20} />
                                </button>
                            </div>
                            <div className="modal-content">
                                {/* Date Range */}
                                <div className="filter-section">
                                    <h3>Date Range</h3>
                                    <div className="date-grid">
                                        <div className="form-group">
                                            <label>From Date</label>
                                            <input
                                                type="date"
                                                value={advancedFilters.dateFrom}
                                                onChange={(e) => setAdvancedFilters(prev => ({ ...prev, dateFrom: e.target.value }))}
                                            />
                                        </div>
                                        <div className="form-group">
                                            <label>To Date</label>
                                            <input
                                                type="date"
                                                value={advancedFilters.dateTo}
                                                onChange={(e) => setAdvancedFilters(prev => ({ ...prev, dateTo: e.target.value }))}
                                            />
                                        </div>
                                    </div>
                                </div>

                                {/* Status Filter */}
                                <div className="filter-section">
                                    <h3>Status</h3>
                                    <div className="checkbox-grid">
                                        {Object.keys(advancedFilters.status).map(status => (
                                            <label key={status}>
                                                <input
                                                    type="checkbox"
                                                    checked={advancedFilters.status[status]}
                                                    onChange={(e) => handleAdvancedFilterChange('status', status, e.target.checked)}
                                                />
                                                <span>{status}</span>
                                            </label>
                                        ))}
                                    </div>
                                </div>

                                {/* Other filter sections */}
                            </div>
                            <div className="modal-footer">
                                <button className="secondary">Clear All</button>
                                <button className="primary">Apply Filters</button>
                            </div>
                        </div>
                    </div>
                )}
            </div>
        </div>
    );
};

export default LeadManagement;
```

## Key Responsive Features

### 1. Breakpoints
The SCSS uses these breakpoints:
- **Mobile**: up to 480px
- **Tablet**: 481px to 768px  
- **Desktop**: 769px to 1024px
- **Large**: 1025px and above

### 2. Mobile-First Approach
The styles are written with a mobile-first approach, meaning:
- Base styles target mobile devices
- Media queries add styles for larger screens
- Progressive enhancement for better performance

### 3. Table Responsiveness
On smaller screens:
- Table has horizontal scroll with smooth touch scrolling
- Minimum widths prevent content from becoming unreadable
- Smaller padding and font sizes for better space utilization

### 4. Modal Responsiveness
- Modals automatically adjust size based on screen size
- On mobile: full-width with proper spacing
- Form grids collapse from 2 columns to 1 column on mobile
- Buttons stack vertically on mobile

### 5. Control Panel Adaptations
- Search and filter controls stack vertically on tablets
- Buttons become full-width on mobile
- Filter dropdowns become full-width for better touch targets

## Optional: Mobile Card View

The SCSS includes styles for an optional mobile card view. To implement this, you can add a toggle button and alternative rendering:

```jsx
// Add state for view mode
const [isMobileCardView, setIsMobileCardView] = useState(false);

// Add toggle button in controls
<button 
    className="hidden-desktop"
    onClick={() => setIsMobileCardView(!isMobileCardView)}
>
    {isMobileCardView ? 'Table View' : 'Card View'}
</button>

// Conditional rendering
{isMobileCardView ? (
    <div className="mobile-card-view active">
        {filteredLeads.slice(0, viewMode).map((lead) => (
            <div key={lead.id} className="lead-card">
                <div className="card-header">
                    <span className="lead-id">{lead.id}</span>
                    <span className={`status-badge ${statusColors[lead.status]}`}>
                        {lead.status}
                    </span>
                </div>
                <div className="card-content">
                    <div className="client-info">
                        <div className="avatar">
                            {lead.clientInfo.name.charAt(0)}
                        </div>
                        <div className="info">
                            <div className="name">{lead.clientInfo.name}</div>
                            <div className="phone">
                                <Phone size={12} />
                                {lead.clientInfo.phone}
                            </div>
                        </div>
                    </div>
                    <div className="card-details">
                        <div className="detail-item">
                            <div className="label">City</div>
                            <div className="value">{lead.city}</div>
                        </div>
                        <div className="detail-item">
                            <div className="label">Service</div>
                            <div className="value">{lead.serviceType}</div>
                        </div>
                        <div className="detail-item">
                            <div className="label">Budget</div>
                            <div className="value">{lead.budget}</div>
                        </div>
                        <div className="detail-item">
                            <div className="label">Assigned To</div>
                            <div className="value">{lead.assignedTo}</div>
                        </div>
                    </div>
                </div>
                <div className="card-actions">
                    {/* Action buttons */}
                </div>
            </div>
        ))}
    </div>
) : (
    <div className={`table-container ${isMobileCardView ? 'hidden-mobile' : ''}`}>
        {/* Your table content */}
    </div>
)}
```

## Additional Features

### Print Styles
The SCSS includes print-friendly styles that:
- Hide interactive elements (buttons, modals)
- Optimize table layout for printing
- Use high contrast borders

### Dark Mode Support
Optional dark mode styles are included. To enable:
1. Add `dark-mode` class to the main container
2. Implement a theme toggle mechanism
3. Store user preference in localStorage

### High DPI Display Support
The styles include optimizations for high-resolution displays to ensure crisp rendering of UI elements.

## Browser Compatibility
The SCSS uses modern CSS features but maintains compatibility with:
- Chrome 60+
- Firefox 55+
- Safari 12+
- Edge 79+

For older browser support, consider adding vendor prefixes using autoprefixer.

## Performance Tips
1. The SCSS is optimized for tree-shaking when using modern build tools
2. Consider lazy-loading the SCSS for non-critical components
3. Use CSS containment for better rendering performance:
   ```scss
   .table-container {
     contain: layout style paint;
   }
   ```

## Customization
To customize the styles:
1. Modify the SCSS variables at the top of the file
2. Override specific selectors as needed
3. Add custom breakpoints if required

The modular structure makes it easy to customize specific sections without affecting others.