# CampusShield
import React, { useState } from 'react';
import { AlertCircle, Send, Clock, CheckCircle, Eye, EyeOff, Shield, Users, BookOpen, Heart, AlertTriangle, Search, Filter, Calendar, User } from 'lucide-react';

const StudentReportingPlatform = () => {
  const [currentView, setCurrentView] = useState('report');
  const [reports, setReports] = useState([
    {
      id: 'RPT-001',
      type: 'harassment',
      severity: 'high',
      status: 'investigating',
      title: 'Inappropriate behavior in dormitory',
      description: 'Ongoing harassment from senior students in Building A',
      submittedAt: '2025-07-15',
      lastUpdated: '2025-07-17',
      department: 'Student Affairs',
      anonymous: true
    },
    {
      id: 'RPT-002',
      type: 'ragging',
      severity: 'critical',
      status: 'escalated',
      title: 'Ragging incident at mess hall',
      description: 'First-year students being forced to perform degrading tasks',
      submittedAt: '2025-07-10',
      lastUpdated: '2025-07-18',
      department: 'Disciplinary Committee',
      anonymous: true
    },
    {
      id: 'RPT-003',
      type: 'discrimination',
      severity: 'medium',
      status: 'resolved',
      title: 'Discriminatory remarks in classroom',
      description: 'Professor making biased comments about certain communities',
      submittedAt: '2025-07-05',
      lastUpdated: '2025-07-16',
      department: 'Academic Affairs',
      anonymous: false
    }
  ]);

  const [formData, setFormData] = useState({
    type: '',
    severity: '',
    title: '',
    description: '',
    location: '',
    datetime: '',
    witnesses: '',
    anonymous: true,
    contactInfo: ''
  });

  const [trackingId, setTrackingId] = useState('');
  const [searchFilter, setSearchFilter] = useState({ type: '', status: '', severity: '' });

  const issueTypes = [
    { value: 'harassment', label: 'Harassment', icon: AlertTriangle, color: 'text-red-600' },
    { value: 'ragging', label: 'Ragging/Bullying', icon: Users, color: 'text-red-700' },
    { value: 'discrimination', label: 'Discrimination', icon: Users, color: 'text-orange-600' },
    { value: 'academic', label: 'Academic Misconduct', icon: BookOpen, color: 'text-blue-600' },
    { value: 'safety', label: 'Safety Concerns', icon: Shield, color: 'text-yellow-600' },
    { value: 'mental_health', label: 'Mental Health Support', icon: Heart, color: 'text-purple-600' }
  ];

  const severityLevels = [
    { value: 'low', label: 'Low Priority', color: 'bg-green-100 text-green-800' },
    { value: 'medium', label: 'Medium Priority', color: 'bg-yellow-100 text-yellow-800' },
    { value: 'high', label: 'High Priority', color: 'bg-orange-100 text-orange-800' },
    { value: 'critical', label: 'Critical/Urgent', color: 'bg-red-100 text-red-800' }
  ];

  const statusTypes = [
    { value: 'submitted', label: 'Submitted', color: 'bg-blue-100 text-blue-800' },
    { value: 'reviewing', label: 'Under Review', color: 'bg-yellow-100 text-yellow-800' },
    { value: 'investigating', label: 'Investigating', color: 'bg-orange-100 text-orange-800' },
    { value: 'escalated', label: 'Escalated', color: 'bg-red-100 text-red-800' },
    { value: 'resolved', label: 'Resolved', color: 'bg-green-100 text-green-800' },
    { value: 'closed', label: 'Closed', color: 'bg-gray-100 text-gray-800' }
  ];

  const handleSubmit = () => {
    if (!formData.type || !formData.severity || !formData.title || !formData.description) {
      alert('Please fill in all required fields');
      return;
    }
    const newReport = {
      id: `RPT-${String(reports.length + 1).padStart(3, '0')}`,
      ...formData,
      submittedAt: new Date().toISOString().split('T')[0],
      lastUpdated: new Date().toISOString().split('T')[0],
      status: 'submitted',
      department: formData.type === 'academic' ? 'Academic Affairs' : 
                  formData.type === 'ragging' ? 'Disciplinary Committee' : 'Student Affairs'
    };
    
    setReports([...reports, newReport]);
    setFormData({
      type: '',
      severity: '',
      title: '',
      description: '',
      location: '',
      datetime: '',
      witnesses: '',
      anonymous: true,
      contactInfo: ''
    });
    
    alert(`Report submitted successfully! Your tracking ID is: ${newReport.id}`);
  };

  const getStatusColor = (status) => {
    const statusObj = statusTypes.find(s => s.value === status);
    return statusObj ? statusObj.color : 'bg-gray-100 text-gray-800';
  };

  const getSeverityColor = (severity) => {
    const severityObj = severityLevels.find(s => s.value === severity);
    return severityObj ? severityObj.color : 'bg-gray-100 text-gray-800';
  };

  const filteredReports = reports.filter(report => {
    const matchesType = !searchFilter.type || report.type === searchFilter.type;
    const matchesStatus = !searchFilter.status || report.status === searchFilter.status;
    const matchesSeverity = !searchFilter.severity || report.severity === searchFilter.severity;
    const matchesTracking = !trackingId || report.id.toLowerCase().includes(trackingId.toLowerCase());
    
    return matchesType && matchesStatus && matchesSeverity && matchesTracking;
  });

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100">
      {/* Header */}
      <div className="bg-white shadow-lg border-b-4 border-indigo-600">
        <div className="max-w-7xl mx-auto px-4 py-6">
          <div className="flex items-center justify-between">
            <div className="flex items-center space-x-3">
              <Shield className="w-8 h-8 text-indigo-600" />
              <div>
                <h1 className="text-2xl font-bold text-gray-900">SafeSpeak</h1>
                <p className="text-sm text-gray-600">Anonymous Student Reporting Platform</p>
              </div>
            </div>
            <div className="flex space-x-2">
              <button
                onClick={() => setCurrentView('report')}
                className={`px-4 py-2 rounded-lg font-medium transition-colors ${
                  currentView === 'report' 
                    ? 'bg-indigo-600 text-white' 
                    : 'bg-gray-100 text-gray-700 hover:bg-gray-200'
                }`}
              >
                Submit Report
              </button>
              <button
                onClick={() => setCurrentView('track')}
                className={`px-4 py-2 rounded-lg font-medium transition-colors ${
                  currentView === 'track' 
                    ? 'bg-indigo-600 text-white' 
                    : 'bg-gray-100 text-gray-700 hover:bg-gray-200'
                }`}
              >
                Track Reports
              </button>
              <button
                onClick={() => setCurrentView('dashboard')}
                className={`px-4 py-2 rounded-lg font-medium transition-colors ${
                  currentView === 'dashboard' 
                    ? 'bg-indigo-600 text-white' 
                    : 'bg-gray-100 text-gray-700 hover:bg-gray-200'
                }`}
              >
                Admin Dashboard
              </button>
            </div>
          </div>
        </div>
      </div>

      <div className="max-w-7xl mx-auto px-4 py-8">
        {/* Submit Report View */}
        {currentView === 'report' && (
          <div className="max-w-4xl mx-auto">
            <div className="bg-white rounded-xl shadow-xl p-8">
              <div className="mb-8">
                <h2 className="text-3xl font-bold text-gray-900 mb-2">Submit a Report</h2>
                <p className="text-gray-600">Your privacy and safety are our priority. All reports are handled confidentially.</p>
              </div>

              {/* Issue Type Selection */}
              <div className="mb-8">
                <label className="block text-lg font-semibold text-gray-700 mb-4">What type of issue are you reporting?</label>
                <div className="grid grid-cols-2 md:grid-cols-3 gap-4">
                  {issueTypes.map((type) => (
                    <button
                      key={type.value}
                      onClick={() => setFormData({...formData, type: type.value})}
                      className={`p-4 rounded-lg border-2 transition-all ${
                        formData.type === type.value
                          ? 'border-indigo-500 bg-indigo-50'
                          : 'border-gray-200 hover:border-gray-300'
                      }`}
                    >
                      <type.icon className={`w-6 h-6 mb-2 ${type.color}`} />
                      <div className="font-medium text-gray-900">{type.label}</div>
                    </button>
                  ))}
                </div>
              </div>

              <div className="space-y-6">
                {/* Severity Level */}
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">Priority Level</label>
                  <select
                    value={formData.severity}
                    onChange={(e) => setFormData({...formData, severity: e.target.value})}
                    className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500"
                    required
                  >
                    <option value="">Select priority level</option>
                    {severityLevels.map((level) => (
                      <option key={level.value} value={level.value}>{level.label}</option>
                    ))}
                  </select>
                </div>

                {/* Title */}
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">Report Title</label>
                  <input
                    type="text"
                    value={formData.title}
                    onChange={(e) => setFormData({...formData, title: e.target.value})}
                    className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500"
                    placeholder="Brief description of the issue"
                    required
                  />
                </div>

                {/* Description */}
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">Detailed Description</label>
                  <textarea
                    value={formData.description}
                    onChange={(e) => setFormData({...formData, description: e.target.value})}
                    rows={6}
                    className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500"
                    placeholder="Please provide as much detail as possible about the incident..."
                    required
                  />
                </div>

                {/* Location and Time */}
                <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-2">Location</label>
                    <input
                      type="text"
                      value={formData.location}
                      onChange={(e) => setFormData({...formData, location: e.target.value})}
                      className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500"
                      placeholder="Where did this occur?"
                    />
                  </div>
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-2">Date & Time</label>
                    <input
                      type="datetime-local"
                      value={formData.datetime}
                      onChange={(e) => setFormData({...formData, datetime: e.target.value})}
                      className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500"
                    />
                  </div>
                </div>

                {/* Witnesses */}
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-2">Witnesses (Optional)</label>
                  <textarea
                    value={formData.witnesses}
                    onChange={(e) => setFormData({...formData, witnesses: e.target.value})}
                    rows={3}
                    className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500"
                    placeholder="Were there any witnesses? (Names, descriptions, etc.)"
                  />
                </div>

                {/* Anonymous Toggle */}
                <div className="bg-gray-50 p-4 rounded-lg">
                  <div className="flex items-center justify-between">
                    <div className="flex items-center space-x-3">
                      {formData.anonymous ? <EyeOff className="w-5 h-5 text-indigo-600" /> : <Eye className="w-5 h-5 text-gray-400" />}
                      <div>
                        <p className="font-medium text-gray-900">Submit Anonymously</p>
                        <p className="text-sm text-gray-600">Your identity will be completely protected</p>
                      </div>
                    </div>
                    <button
                      type="button"
                      onClick={() => setFormData({...formData, anonymous: !formData.anonymous})}
                      className={`w-12 h-6 rounded-full transition-colors ${
                        formData.anonymous ? 'bg-indigo-600' : 'bg-gray-300'
                      }`}
                    >
                      <div className={`w-5 h-5 bg-white rounded-full transition-transform ${
                        formData.anonymous ? 'translate-x-6' : 'translate-x-1'
                      }`} />
                    </button>
                  </div>
                </div>

                {/* Contact Info (if not anonymous) */}
                {!formData.anonymous && (
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-2">Contact Information</label>
                    <input
                      type="text"
                      value={formData.contactInfo}
                      onChange={(e) => setFormData({...formData, contactInfo: e.target.value})}
                      className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500"
                      placeholder="Email or phone number for follow-up"
                    />
                  </div>
                )}

                {/* Submit Button */}
                <button
                  onClick={handleSubmit}
                  className="w-full bg-indigo-600 text-white py-4 rounded-lg font-semibold hover:bg-indigo-700 transition-colors flex items-center justify-center space-x-2"
                >
                  <Send className="w-5 h-5" />
                  <span>Submit Report</span>
                </button>
              </div>
            </div>
          </div>
        )}

        {/* Track Reports View */}
        {currentView === 'track' && (
          <div className="max-w-4xl mx-auto">
            <div className="bg-white rounded-xl shadow-xl p-8">
              <h2 className="text-3xl font-bold text-gray-900 mb-6">Track Your Report</h2>
              
              <div className="mb-6">
                <label className="block text-sm font-medium text-gray-700 mb-2">Enter Tracking ID</label>
                <div className="flex space-x-4">
                  <input
                    type="text"
                    value={trackingId}
                    onChange={(e) => setTrackingId(e.target.value)}
                    className="flex-1 p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500"
                    placeholder="e.g., RPT-001"
                  />
                  <button className="bg-indigo-600 text-white px-6 py-3 rounded-lg hover:bg-indigo-700 transition-colors">
                    <Search className="w-5 h-5" />
                  </button>
                </div>
              </div>

              {/* Report Results */}
              {filteredReports.length > 0 && (
                <div className="space-y-4">
                  {filteredReports.map((report) => (
                    <div key={report.id} className="border border-gray-200 rounded-lg p-6">
                      <div className="flex justify-between items-start mb-4">
                        <div>
                          <h3 className="text-lg font-semibold text-gray-900">{report.title}</h3>
                          <p className="text-sm text-gray-600">ID: {report.id}</p>
                        </div>
                        <div className="flex space-x-2">
                          <span className={`px-3 py-1 rounded-full text-sm font-medium ${getSeverityColor(report.severity)}`}>
                            {severityLevels.find(s => s.value === report.severity)?.label}
                          </span>
                          <span className={`px-3 py-1 rounded-full text-sm font-medium ${getStatusColor(report.status)}`}>
                            {statusTypes.find(s => s.value === report.status)?.label}
                          </span>
                        </div>
                      </div>
                      
                      <div className="grid grid-cols-1 md:grid-cols-3 gap-4 text-sm">
                        <div>
                          <p className="font-medium text-gray-700">Submitted</p>
                          <p className="text-gray-600">{report.submittedAt}</p>
                        </div>
                        <div>
                          <p className="font-medium text-gray-700">Last Updated</p>
                          <p className="text-gray-600">{report.lastUpdated}</p>
                        </div>
                        <div>
                          <p className="font-medium text-gray-700">Department</p>
                          <p className="text-gray-600">{report.department}</p>
                        </div>
                      </div>
                    </div>
                  ))}
                </div>
              )}
            </div>
          </div>
        )}

        {/* Admin Dashboard View */}
        {currentView === 'dashboard' && (
          <div>
            <div className="bg-white rounded-xl shadow-xl p-8 mb-8">
              <h2 className="text-3xl font-bold text-gray-900 mb-6">Admin Dashboard</h2>
              
              {/* Stats */}
              <div className="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
                <div className="bg-blue-50 p-6 rounded-lg">
                  <div className="flex items-center justify-between">
                    <div>
                      <p className="text-blue-600 font-medium">Total Reports</p>
                      <p className="text-3xl font-bold text-blue-900">{reports.length}</p>
                    </div>
                    <AlertCircle className="w-8 h-8 text-blue-600" />
                  </div>
                </div>
                
                <div className="bg-yellow-50 p-6 rounded-lg">
                  <div className="flex items-center justify-between">
                    <div>
                      <p className="text-yellow-600 font-medium">Pending</p>
                      <p className="text-3xl font-bold text-yellow-900">
                        {reports.filter(r => ['submitted', 'reviewing', 'investigating'].includes(r.status)).length}
                      </p>
                    </div>
                    <Clock className="w-8 h-8 text-yellow-600" />
                  </div>
                </div>
                
                <div className="bg-red-50 p-6 rounded-lg">
                  <div className="flex items-center justify-between">
                    <div>
                      <p className="text-red-600 font-medium">Critical</p>
                      <p className="text-3xl font-bold text-red-900">
                        {reports.filter(r => r.severity === 'critical').length}
                      </p>
                    </div>
                    <AlertTriangle className="w-8 h-8 text-red-600" />
                  </div>
                </div>
                
                <div className="bg-green-50 p-6 rounded-lg">
                  <div className="flex items-center justify-between">
                    <div>
                      <p className="text-green-600 font-medium">Resolved</p>
                      <p className="text-3xl font-bold text-green-900">
                        {reports.filter(r => r.status === 'resolved').length}
                      </p>
                    </div>
                    <CheckCircle className="w-8 h-8 text-green-600" />
                  </div>
                </div>
              </div>

              {/* Filters */}
              <div className="flex flex-wrap gap-4 mb-6">
                <select
                  value={searchFilter.type}
                  onChange={(e) => setSearchFilter({...searchFilter, type: e.target.value})}
                  className="p-2 border border-gray-300 rounded-lg"
                >
                  <option value="">All Types</option>
                  {issueTypes.map((type) => (
                    <option key={type.value} value={type.value}>{type.label}</option>
                  ))}
                </select>
                
                <select
                  value={searchFilter.status}
                  onChange={(e) => setSearchFilter({...searchFilter, status: e.target.value})}
                  className="p-2 border border-gray-300 rounded-lg"
                >
                  <option value="">All Status</option>
                  {statusTypes.map((status) => (
                    <option key={status.value} value={status.value}>{status.label}</option>
                  ))}
                </select>
                
                <select
                  value={searchFilter.severity}
                  onChange={(e) => setSearchFilter({...searchFilter, severity: e.target.value})}
                  className="p-2 border border-gray-300 rounded-lg"
                >
                  <option value="">All Priorities</option>
                  {severityLevels.map((level) => (
                    <option key={level.value} value={level.value}>{level.label}</option>
                  ))}
                </select>
              </div>

              {/* Reports Table */}
              <div className="overflow-x-auto">
                <table className="w-full">
                  <thead>
                    <tr className="border-b border-gray-200">
                      <th className="text-left py-3 px-4 font-medium text-gray-700">ID</th>
                      <th className="text-left py-3 px-4 font-medium text-gray-700">Title</th>
                      <th className="text-left py-3 px-4 font-medium text-gray-700">Type</th>
                      <th className="text-left py-3 px-4 font-medium text-gray-700">Severity</th>
                      <th className="text-left py-3 px-4 font-medium text-gray-700">Status</th>
                      <th className="text-left py-3 px-4 font-medium text-gray-700">Department</th>
                      <th className="text-left py-3 px-4 font-medium text-gray-700">Date</th>
                    </tr>
                  </thead>
                  <tbody>
                    {filteredReports.map((report) => (
                      <tr key={report.id} className="border-b border-gray-100 hover:bg-gray-50">
                        <td className="py-3 px-4 font-medium text-indigo-600">{report.id}</td>
                        <td className="py-3 px-4">{report.title}</td>
                        <td className="py-3 px-4 capitalize">{report.type.replace('_', ' ')}</td>
                        <td className="py-3 px-4">
                          <span className={`px-2 py-1 rounded-full text-xs font-medium ${getSeverityColor(report.severity)}`}>
                            {report.severity}
                          </span>
                        </td>
                        <td className="py-3 px-4">
                          <span className={`px-2 py-1 rounded-full text-xs font-medium ${getStatusColor(report.status)}`}>
                            {report.status}
                          </span>
                        </td>
                        <td className="py-3 px-4">{report.department}</td>
                        <td className="py-3 px-4">{report.submittedAt}</td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        )}
      </div>
    </div>
  );
};

export default StudentReportingPlatform;
