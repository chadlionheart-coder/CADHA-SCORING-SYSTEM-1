# CADHA-SCORING-SYSTEM-1
SCORING
import React, { useState, useEffect } from 'react';
import { Plus, Award, Clock, AlertTriangle, User, UserCheck, Target, Ban, Hash, LogIn, LogOut, CheckCircle, X, ChevronsDown, ChevronsUp, Edit, Send, Trash2, LayoutGrid, ClipboardCheck } from 'lucide-react';

// Mock Firebase configuration for demo purposes
const mockFirebaseConfig = {
  apiKey: "demo-key",
  authDomain: "demo.firebaseapp.com",
  projectId: "demo-project",
  storageBucket: "demo.appspot.com",
  messagingSenderId: "123456789",
  appId: "demo-app-id"
};

// Pre-defined divisions and categories
const DIVISIONS = [
  "D.P – DEFENDER PRODUCTION",
  "D.S – DEFENDER STANDARD",
  "D.R – DEFENDER REVOLVER",
  "D.S.S – DEFENDER SINGLE STACK",
  "D.C.O – DEFENDER CARRY OPTICS",
  "D.C.C – DEFENSIVE CONCEALED CARRY"
];

const CATEGORIES = [
  "JUNIOR",
  "LADY",
  "SENIOR",
  "SUPER SENIOR",
  "NALEC/LAWMAN IN UNIFORM",
  "None"
];

// Helper component for a custom modal
const Modal = ({ title, children, onClose }) => (
  <div className="fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center p-4 z-50">
    <div className="bg-white dark:bg-gray-800 p-8 rounded-2xl shadow-2xl w-full max-w-4xl max-h-[90vh] overflow-y-auto">
      <div className="flex justify-between items-center mb-6">
        <h3 className="text-2xl font-bold text-gray-900 dark:text-gray-100">{title}</h3>
        <button onClick={onClose} className="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300 transition-colors">
          <X size={24} />
        </button>
      </div>
      {children}
    </div>
  </div>
);

// Form component for adding shooters and stages
const InputForm = ({ type, onSubmit, onCancel }) => {
  const [formData, setFormData] = useState({
    name: '',
    division: DIVISIONS[0],
    category: CATEGORIES[0],
    threatTargets: 0,
    nonThreatTargets: 0,
    totalRounds: 0
  });

  const handleInputChange = (e) => {
    const { name, value, type: inputType } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: inputType === 'number' ? parseInt(value) || 0 : value
    }));
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    const success = await onSubmit(formData);
    if (success) {
      setFormData({
        name: '',
        division: DIVISIONS[0],
        category: CATEGORIES[0],
        threatTargets: 0,
        nonThreatTargets: 0,
        totalRounds: 0
      });
      onCancel();
    }
  };

  return (
    <div className="max-w-2xl mx-auto p-6 bg-gray-800 rounded-xl shadow-lg">
      <h2 className="text-2xl font-bold mb-6 text-cyan-400">
        Add New {type === 'shooter' ? 'Shooter' : 'Stage'}
      </h2>
      <form onSubmit={handleSubmit} className="space-y-4">
        <div>
          <label className="block text-sm font-medium text-gray-300 mb-2">
            {type === 'shooter' ? 'Shooter Name' : 'Stage Name'}
          </label>
          <input
            type="text"
            name="name"
            value={formData.name}
            onChange={handleInputChange}
            required
            className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
            placeholder={type === 'shooter' ? 'Enter shooter name' : 'Enter stage name'}
          />
        </div>

        {type === 'shooter' && (
          <>
            <div>
              <label className="block text-sm font-medium text-gray-300 mb-2">Division</label>
              <select
                name="division"
                value={formData.division}
                onChange={handleInputChange}
                className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
              >
                {DIVISIONS.map(division => (
                  <option key={division} value={division}>{division}</option>
                ))}
              </select>
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-300 mb-2">Category</label>
              <select
                name="category"
                value={formData.category}
                onChange={handleInputChange}
                className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
              >
                {CATEGORIES.map(category => (
                  <option key={category} value={category}>{category}</option>
                ))}
              </select>
            </div>
          </>
        )}

        {type === 'stage' && (
          <div className="grid grid-cols-3 gap-4">
            <div>
              <label className="block text-sm font-medium text-gray-300 mb-2">Threat Targets</label>
              <input
                type="number"
                name="threatTargets"
                value={formData.threatTargets}
                onChange={handleInputChange}
                min="0"
                className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-300 mb-2">Non-Threat Targets</label>
              <input
                type="number"
                name="nonThreatTargets"
                value={formData.nonThreatTargets}
                onChange={handleInputChange}
                min="0"
                className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
              />
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-300 mb-2">Total Rounds</label>
              <input
                type="number"
                name="totalRounds"
                value={formData.totalRounds}
                onChange={handleInputChange}
                min="0"
                className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
              />
            </div>
          </div>
        )}

        <div className="flex gap-4 pt-4">
          <button
            type="submit"
            className="flex-1 bg-cyan-500 hover:bg-cyan-600 text-white font-bold py-3 px-6 rounded-lg transition-colors"
          >
            Add {type === 'shooter' ? 'Shooter' : 'Stage'}
          </button>
          <button
            type="button"
            onClick={onCancel}
            className="flex-1 bg-gray-600 hover:bg-gray-700 text-white font-bold py-3 px-6 rounded-lg transition-colors"
          >
            Cancel
          </button>
        </div>
      </form>
    </div>
  );
};

// New component for editing shooters and stages
const EditModal = ({ item, type, onClose, onEdit }) => {
  const [formData, setFormData] = useState(item);

  const handleInputChange = (e) => {
    const { name, value, type: inputType } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: inputType === 'number' ? parseInt(value) || 0 : value
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    onEdit(formData);
    onClose();
  };

  return (
    <Modal title={`Edit ${type === 'shooter' ? 'Shooter' : 'Stage'}`} onClose={onClose}>
      <form onSubmit={handleSubmit} className="space-y-4">
        <div>
          <label className="block text-sm font-medium text-gray-300 mb-2">
            {type === 'shooter' ? 'Shooter Name' : 'Stage Name'}
          </label>
          <input
            type="text"
            name="name"
            value={formData.name}
            onChange={handleInputChange}
            required
            className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
          />
        </div>

        {type === 'shooter' && (
          <>
            <div>
              <label className="block text-sm font-medium text-gray-300 mb-2">Division</label>
              <select
                name="division"
                value={formData.division}
                onChange={handleInputChange}
                className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
              >
                {DIVISIONS.map(division => (
                  <option key={division} value={division}>{division}</option>
                ))}
              </select>
            </div>
            <div>
              <label className="block text-sm font-medium text-gray-300 mb-2">Category</label>
              <select
                name="category"
                value={formData.category}
                onChange={handleInputChange}
                className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
              >
                {CATEGORIES.map(category => (
                  <option key={category} value={category}>{category}</option>
                ))}
              </select>
            </div>
          </>
        )}

        {type === 'stage' && (
          <>
            <div>
              <label className="block text-sm font-medium text-gray-300 mb-2">Range Marshal</label>
              <input
                type="text"
                name="rangeMarshal"
                value={formData.rangeMarshal}
                onChange={handleInputChange}
                required
                className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
              />
            </div>
            <div className="grid grid-cols-3 gap-4">
              <div>
                <label className="block text-sm font-medium text-gray-300 mb-2">Threat Targets</label>
                <input
                  type="number"
                  name="threatTargets"
                  value={formData.threatTargets}
                  onChange={handleInputChange}
                  min="0"
                  className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
                />
              </div>
              <div>
                <label className="block text-sm font-medium text-gray-300 mb-2">Non-Threat Targets</label>
                <input
                  type="number"
                  name="nonThreatTargets"
                  value={formData.nonThreatTargets}
                  onChange={handleInputChange}
                  min="0"
                  className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
                />
              </div>
              <div>
                <label className="block text-sm font-medium text-gray-300 mb-2">Total Rounds</label>
                <input
                  type="number"
                  name="totalRounds"
                  value={formData.totalRounds}
                  onChange={handleInputChange}
                  min="0"
                  className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none"
                />
              </div>
            </div>
          </>
        )}

        <div className="flex gap-4 pt-4">
          <button
            type="submit"
            className="flex-1 bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-lg transition-colors"
          >
            Save Changes
          </button>
          <button
            type="button"
            onClick={onClose}
            className="flex-1 bg-gray-600 hover:bg-gray-700 text-white font-bold py-3 px-6 rounded-lg transition-colors"
          >
            Cancel
          </button>
        </div>
      </form>
    </Modal>
  );
};

// NEW: Component for assigning stages to range marshals (Admin only)
// Now includes a button to add new stages
const AssignStagesModal = ({ stages, onSave, onCancel, onAddStage }) => {
  const [assignments, setAssignments] = useState(() =>
    stages.map(stage => ({
      id: stage.id,
      name: stage.name,
      rangeMarshal: stage.rangeMarshal || ''
    }))
  );
  const [showAddStageForm, setShowAddStageForm] = useState(false);

  // Sync internal state with props when stages change
  useEffect(() => {
    setAssignments(stages.map(stage => ({
      id: stage.id,
      name: stage.name,
      rangeMarshal: stage.rangeMarshal || ''
    })));
  }, [stages]);

  // Handle manual input of Range Marshal name
  const handleAssignmentChange = (stageId, rmName) => {
    setAssignments(prev => prev.map(assignment =>
      assignment.id === stageId ? { ...assignment, rangeMarshal: rmName } : assignment
    ));
  };

  const handleSave = () => {
    onSave(assignments);
    onCancel();
  };

  return (
    <Modal title={showAddStageForm ? "Add New Stage" : "Assign Stages to Range Marshals"} onClose={onCancel}>
      {!showAddStageForm ? (
        <>
          <div className="space-y-4">
            <div className="flex justify-end">
              <button
                onClick={() => setShowAddStageForm(true)}
                className="bg-cyan-500 hover:bg-cyan-600 text-white font-bold py-2 px-4 rounded-lg flex items-center transition-colors"
              >
                <Plus size={16} className="mr-2" />
                Add New Stage
              </button>
            </div>
            {assignments.length > 0 ? (
              assignments.map(assignment => (
                <div key={assignment.id} className="flex flex-col sm:flex-row items-start sm:items-center space-y-2 sm:space-y-0 sm:space-x-4 p-2 border-b border-gray-700 last:border-b-0">
                  <p className="flex-1 text-lg font-semibold text-gray-300">{assignment.name}</p>
                  <input
                    type="text"
                    value={assignment.rangeMarshal}
                    onChange={(e) => handleAssignmentChange(assignment.id, e.target.value)}
                    className="w-full sm:flex-1 p-2 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400"
                    placeholder="Enter Range Marshal Name"
                  />
                </div>
              ))
            ) : (
              <p className="text-center text-gray-400 italic">No stages available to assign. Add one first!</p>
            )}
          </div>
          <div className="mt-6 flex justify-end gap-4">
            <button
              onClick={handleSave}
              className="bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded-lg transition-colors"
            >
              Save Assignments
            </button>
            <button
              onClick={onCancel}
              className="bg-gray-600 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded-lg transition-colors"
            >
              Cancel
            </button>
          </div>
        </>
      ) : (
        <InputForm
          type="stage"
          onSubmit={onAddStage}
          onCancel={() => setShowAddStageForm(false)}
        />
      )}
    </Modal>
  );
};


// New component for scoring form within the modal
const ScoringModal = ({ shooter, stages, onClose, onSubmitScore, loggedInRangeMarshal }) => {
  const [selectedStageId, setSelectedStageId] = useState('');
  const [scoreData, setScoreData] = useState({
    rawTime: '',
    hits: { zone0: 0, zone2: 0, zone4: 0 },
    penalties: { FTI: false, TNN: 0, TNE: 0, HNT: 0, PE: 0, miss: 0 }
  });

  // Get the stage assigned to the logged-in range marshal
  const assignedStage = stages.find(s => s.rangeMarshal === loggedInRangeMarshal);

  useEffect(() => {
    // If a range marshal is logged in and has an assigned stage, pre-select it
    const stageIdToUse = loggedInRangeMarshal && assignedStage ? assignedStage.id : stages[0]?.id || '';
    setSelectedStageId(stageIdToUse);

    if (shooter && stageIdToUse) {
      // Find the existing score for the selected stage and populate the form
      const currentScore = shooter.scores?.find(s => s.stageId === stageIdToUse) || {
        rawTime: '',
        hits: { zone0: 0, zone2: 0, zone4: 0 },
        penalties: { FTI: false, TNN: 0, TNE: 0, HNT: 0, PE: 0, miss: 0 }
      };
      setScoreData(currentScore);
    }
  }, [shooter, stages, loggedInRangeMarshal, assignedStage]);


  const handleInputChange = (e) => {
    const { name, value, type, checked } = e.target;
    const [field, subfield] = name.split('.');

    if (subfield) {
      setScoreData(prev => ({
        ...prev,
        [field]: {
          ...prev[field],
          [subfield]: type === 'checkbox' ? checked : parseInt(value) || 0
        }
      }));
    } else {
      setScoreData(prev => ({
        ...prev,
        [field]: type === 'number' ? parseFloat(value) || 0 : value
      }));
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!scoreData.rawTime || parseFloat(scoreData.rawTime) <= 0) {
      // Use a custom message box instead of alert()
      const messageBox = document.createElement('div');
      messageBox.textContent = 'Please enter a valid raw time';
      messageBox.className = 'fixed top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-red-500 text-white p-4 rounded-lg shadow-lg z-[100]';
      document.body.appendChild(messageBox);
      setTimeout(() => messageBox.remove(), 3000);
      return;
    }
    onSubmitScore(shooter.id, selectedStageId, scoreData);
    onClose();
  };

  if (!shooter) return null;

  return (
    <Modal title={`Scoring for ${shooter.name}`} onClose={onClose}>
      <form onSubmit={handleSubmit}>
        <div className="mb-4">
          <label className="block text-sm font-medium text-gray-400 mb-2">Select Stage</label>
          <select
            value={selectedStageId}
            onChange={(e) => setSelectedStageId(e.target.value)}
            className="w-full p-2 bg-gray-700 text-white rounded-md border border-gray-600"
            disabled={!!loggedInRangeMarshal && !!assignedStage} // Disable if RM is logged in and has an assigned stage
          >
            {/* Only show the assigned stage if RM is logged in */}
            {loggedInRangeMarshal && assignedStage ? (
              <option value={assignedStage.id}>{assignedStage.name}</option>
            ) : (
              stages.map(stage => (
                <option key={stage.id} value={stage.id}>{stage.name}</option>
              ))
            )}
          </select>
        </div>
        <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <h4 className="text-lg font-semibold text-cyan-400 mb-2">Hits & Time</h4>
            <div className="space-y-2">
              <label className="block">
                <span className="text-gray-300">Raw Time (seconds)</span>
                <input
                  type="number"
                  step="0.01"
                  name="rawTime"
                  value={scoreData.rawTime || ''}
                  onChange={handleInputChange}
                  className="w-full p-2 bg-gray-700 text-white rounded-md border border-gray-600 mt-1"
                  required
                />
              </label>
              <label className="block">
                <span className="text-gray-300">0-pt Hits</span>
                <input
                  type="number"
                  name="hits.zone0"
                  value={scoreData.hits?.zone0 || 0}
                  onChange={handleInputChange}
                  className="w-full p-2 bg-gray-700 text-white rounded-md border border-gray-600 mt-1"
                  min="0"
                />
              </label>
              <label className="block">
                <span className="text-gray-300">2-pt Hits</span>
                <input
                  type="number"
                  name="hits.zone2"
                  value={scoreData.hits?.zone2 || 0}
                  onChange={handleInputChange}
                  className="w-full p-2 bg-gray-700 text-white rounded-md border border-gray-600 mt-1"
                  min="0"
                />
              </label>
              <label className="block">
                <span className="text-gray-300">4-pt Hits</span>
                <input
                  type="number"
                  name="hits.zone4"
                  value={scoreData.hits?.zone4 || 0}
                  onChange={handleInputChange}
                  className="w-full p-2 bg-gray-700 text-white rounded-md border border-gray-600 mt-1"
                  min="0"
                />
              </label>
            </div>
          </div>
          <div>
            <h4 className="text-lg font-semibold text-red-400 mb-2">Penalties</h4>
            <div className="space-y-2">
              <label className="block">
                <span className="text-gray-300">Miss (+5s)</span>
                <input
                  type="number"
                  name="penalties.miss"
                  value={scoreData.penalties?.miss || 0}
                  onChange={handleInputChange}
                  className="w-full p-2 bg-gray-700 text-white rounded-md border border-gray-600 mt-1"
                  min="0"
                />
              </label>
              <label className="block">
                <span className="text-gray-300">P.E. (+10s)</span>
                <input
                  type="number"
                  name="penalties.PE"
                  value={scoreData.penalties?.PE || 0}
                  onChange={handleInputChange}
                  className="w-full p-2 bg-gray-700 text-white rounded-md border border-gray-600 mt-1"
                  min="0"
                />
              </label>
              <label className="block">
                <span className="text-gray-300">HNT (+10s)</span>
                <input
                  type="number"
                  name="penalties.HNT"
                  value={scoreData.penalties?.HNT || 0}
                  onChange={handleInputChange}
                  className="w-full p-2 bg-gray-700 text-white rounded-md border border-gray-600 mt-1"
                  min="0"
                />
              </label>
              <label className="block">
                <span className="text-gray-300">TNN (+10s)</span>
                <input
                  type="number"
                  name="penalties.TNN"
                  value={scoreData.penalties?.TNN || 0}
                  onChange={handleInputChange}
                  className="w-full p-2 bg-gray-700 text-white rounded-md border border-gray-600 mt-1"
                  min="0"
                />
              </label>
              <label className="flex items-center space-x-2">
                <input
                  type="checkbox"
                  name="penalties.FTI"
                  checked={scoreData.penalties?.FTI || false}
                  onChange={handleInputChange}
                  className="rounded"
                />
                <span className="text-gray-300">FTI (+20s)</span>
              </label>
              <label className="block">
                <span className="text-gray-300">TNE (+30s)</span>
                <input
                  type="number"
                  name="penalties.TNE"
                  value={scoreData.penalties?.TNE || 0}
                  onChange={handleInputChange}
                  className="w-full p-2 bg-gray-700 text-white rounded-md border border-gray-600 mt-1"
                  min="0"
                />
              </label>
            </div>
          </div>
        </div>
        <div className="mt-6 text-right">
          <button
            type="submit"
            className="bg-cyan-500 hover:bg-cyan-600 text-white font-bold py-2 px-4 rounded-lg flex items-center justify-center ml-auto transition-colors"
          >
            Submit Score
            <Send size={16} className="ml-2" />
          </button>
        </div>
      </form>
    </Modal>
  );
};

// NEW: Component for displaying and editing event details
const EventDetails = ({ eventDetails, onSave, isAdmin }) => {
  const [isEditing, setIsEditing] = useState(false);
  const [formData, setFormData] = useState(eventDetails);

  // Sync internal state with props
  useEffect(() => {
    setFormData(eventDetails);
  }, [eventDetails]);

  // Handle input changes
  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  // Handle saving the details
  const handleSave = () => {
    onSave(formData);
    setIsEditing(false);
  };

  // Read-only view for non-admins
  if (!isAdmin) {
    return (
      <div className="bg-gray-800 p-6 rounded-xl shadow-lg text-white font-inter w-full lg:col-span-3">
        <h2 className="text-3xl font-bold mb-2 text-cyan-400">{eventDetails.title || 'Event Title'}</h2>
        <div className="flex flex-wrap items-center text-gray-300 gap-x-6 gap-y-2 mt-4">
          <div className="flex items-center gap-2">
            <Clock size={18} className="text-cyan-400" />
            <p><strong>Date:</strong> {formData.date || 'N/A'}</p>
          </div>
          <div className="flex items-center gap-2">
            <Clock size={18} className="text-cyan-400" />
            <p><strong>Time:</strong> {formData.time || 'N/A'}</p>
          </div>
          <div className="flex items-center gap-2">
            <Target size={18} className="text-cyan-400" />
            <p><strong>Location:</strong> {formData.location || 'N/A'}</p>
          </div>
        </div>
      </div>
    );
  }

  // Editable view for admins
  return (
    <div className="bg-gray-800 p-6 rounded-xl shadow-lg w-full lg:col-span-3">
      <div className="flex justify-between items-center mb-4">
        <h2 className="text-2xl font-bold text-cyan-400">Event Details</h2>
        {!isEditing && (
          <button
            onClick={() => setIsEditing(true)}
            className="bg-cyan-500 hover:bg-cyan-600 text-white font-bold py-2 px-4 rounded-lg flex items-center transition-colors"
          >
            <Edit size={16} className="mr-2" />
            Edit
          </button>
        )}
      </div>
      <div className="space-y-4">
        <div>
          <label className="block text-sm font-medium text-gray-300 mb-2">Event Title</label>
          <input
            type="text"
            name="title"
            value={formData.title}
            onChange={handleInputChange}
            className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400"
            disabled={!isEditing}
          />
        </div>
        <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
          <div>
            <label className="block text-sm font-medium text-gray-300 mb-2">Date</label>
            <input
              type="date"
              name="date"
              value={formData.date}
              onChange={handleInputChange}
              className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400"
              disabled={!isEditing}
            />
          </div>
          <div>
            <label className="block text-sm font-medium text-gray-300 mb-2">Time</label>
            <input
              type="time"
              name="time"
              value={formData.time}
              onChange={handleInputChange}
              className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400"
              disabled={!isEditing}
            />
          </div>
          <div>
            <label className="block text-sm font-medium text-gray-300 mb-2">Location</label>
            <input
              type="text"
              name="location"
              value={formData.location}
              onChange={handleInputChange}
              className="w-full p-3 bg-gray-700 text-white rounded-lg border border-gray-600 focus:border-cyan-400"
              placeholder="e.g., Range Alpha"
              disabled={!isEditing}
            />
          </div>
        </div>
      </div>
      {isEditing && (
        <div className="flex gap-4 pt-4">
          <button
            onClick={handleSave}
            className="flex-1 bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-lg transition-colors"
          >
            Save
          </button>
          <button
            onClick={() => {
              setIsEditing(false);
              setFormData(eventDetails); // Reset form data
            }}
            className="flex-1 bg-gray-600 hover:bg-gray-700 text-white font-bold py-3 px-6 rounded-lg transition-colors"
          >
            Cancel
          </button>
        </div>
      )}
    </div>
  );
};


// Main Application Component
const App = () => {
  const [shooters, setShooters] = useState([]);
  const [stages, setStages] = useState([]);
  const [pendingScores, setPendingScores] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [adminCode, setAdminCode] = useState('');
  const [isAdmin, setIsAdmin] = useState(false);
  const [loginError, setLoginError] = useState('');

  // State for event details
  const [eventDetails, setEventDetails] = useState({
    title: 'Defensive Shooting Competition',
    date: new Date().toISOString().split('T')[0],
    time: '09:00',
    location: 'Range Alpha'
  });

  // States for the scorekeeper functionality
  const [showScoreInput, setShowScoreInput] = useState(false);
  const [editingScoreForShooter, setEditingScoreForShooter] = useState(null);
  const [leaderboard, setLeaderboard] = useState([]);
  const [loggedInRangeMarshal, setLoggedInRangeMarshal] = useState('');
  const [rangeMarshalLogin, setRangeMarshalLogin] = useState('');
  const [message, setMessage] = useState({ text: '', type: '' });
  const [currentView, setCurrentView] = useState('login');
  const [selectedType, setSelectedType] = useState(null);
  const [showAdminApproval, setShowAdminApproval] = useState(false);

  // New state for admin editing functionality
  const [showEditModal, setShowEditModal] = useState(false);
  const [editingItem, setEditingItem] = useState(null);
  const [editingItemType, setEditingItemType] = useState('');

  // NEW: State for stage assignment modal
  const [showAssignStagesModal, setShowAssignStagesModal] = useState(false);

  // The secret admin code
  const SECRET_ADMIN_CODE = '27ChadLionheart1972$';

  // Function to show a temporary message
  const showMessage = (text, type = 'error') => {
    setMessage({ text, type });
    setTimeout(() => setMessage({ text: '', type: '' }), 3000);
  };

  // Function to handle admin login
  const handleAdminLogin = (e) => {
    e.preventDefault();
    setLoginError('');
    if (adminCode === SECRET_ADMIN_CODE) {
      setIsAdmin(true);
      setLoggedInRangeMarshal(''); // Ensure RM is logged out
      setCurrentView('dashboard');
      showMessage('Admin login successful!', 'success');
    } else {
      setLoginError('Incorrect admin code. Please try again.');
    }
  };

  // Function to handle Range Marshal login
  const handleRangeMarshalLogin = () => {
    if (rangeMarshalLogin.trim() !== '') {
      // Check if the RM name exists on any stage
      const rmExists = stages.some(stage => stage.rangeMarshal === rangeMarshalLogin.trim());
      if (!rmExists) {
        showMessage('Invalid Range Marshal name. Please check with the admin.', 'error');
        return;
      }
      setLoggedInRangeMarshal(rangeMarshalLogin.trim());
      setIsAdmin(false); // Ensure admin is logged out
      showMessage(`Logged in as Range Marshal: ${rangeMarshalLogin}`, 'success');
      setRangeMarshalLogin('');
      setCurrentView('dashboard');
    } else {
      showMessage('Please enter a valid login name.');
    }
  };

  // Function to handle Range Marshal logout
  const handleRangeMarshalLogout = () => {
    setLoggedInRangeMarshal('');
    setIsAdmin(false);
    setCurrentView('login');
    showMessage('Logged out successfully.', 'success');
  };

  // Function to calculate the score for a single shooter and stage
  const calculateShooterScore = (score) => {
    if (!score || !score.rawTime || parseFloat(score.rawTime) <= 0) {
      return { finalScore: 0, totalPoints: 0, totalPenaltyTime: 0 };
    }
    const rawTime = parseFloat(score.rawTime);

    // Calculate total points
    const totalPoints = (score.hits.zone0 * 0) + (score.hits.zone2 * 2) + (score.hits.zone4 * 4);

    // Calculate total penalty time
    let totalPenaltyTime = 0;
    if (score.penalties.FTI) totalPenaltyTime += 20;
    totalPenaltyTime += (score.penalties.TNN || 0) * 10;
    totalPenaltyTime += (score.penalties.TNE || 0) * 30;
    totalPenaltyTime += (score.penalties.HNT || 0) * 10;
    totalPenaltyTime += (score.penalties.PE || 0) * 10;
    totalPenaltyTime += (score.penalties.miss || 0) * 5;

    // Apply the CADHA formula: (Total Points + Penalties) / 2 + Raw Time
    const finalScore = (totalPoints + totalPenaltyTime) / 2 + rawTime;

    return { finalScore, totalPoints, totalPenaltyTime };
  };

  // A helper function to get the stage name from its ID
  const getStageDetails = (stageId) => {
    const stage = stages.find(s => s.id === stageId);
    return stage ? stage : { name: 'Unknown Stage', rangeMarshal: 'N/A', threatTargets: 0, nonThreatTargets: 0 };
  };

  // Generate unique ID for new entries
  const generateId = () => Date.now().toString() + Math.random().toString(36).substr(2, 9);

  // Handle adding a new shooter via the form
  const handleAddShooter = async (newShooterData) => {
    try {
      if (shooters.length >= 80) {
        showMessage('Maximum of 80 shooters reached.', 'error');
        return false;
      }

      // Create a score entry for each existing stage
      const scores = stages.map(stage => ({
        stageId: stage.id,
        rawTime: null,
        hits: { zone0: 0, zone2: 0, zone4: 0 },
        penalties: { FTI: false, TNN: 0, TNE: 0, HNT: 0, PE: 0, miss: 0 }
      }));

      const newShooter = {
        id: generateId(),
        ...newShooterData,
        scores: scores,
        createdAt: new Date(),
        overallScore: 0
      };

      setShooters(prev => [...prev, newShooter]);
      showMessage('Shooter added successfully!', 'success');
      return true;
    } catch (e) {
      console.error("Error adding shooter: ", e);
      showMessage("Failed to add shooter. Please try again.");
      return false;
    }
  };

  // Handle adding a new stage via the form
  const handleAddStage = async (newStageData) => {
    try {
      if (stages.length >= 8) {
        showMessage('Maximum of 8 stages reached.', 'error');
        return false;
      }

      const newStage = {
        id: generateId(),
        ...newStageData,
        createdAt: new Date(),
        rangeMarshal: '' // Initialize range marshal as empty
      };

      // Add the new stage
      setStages(prev => [...prev, newStage]);

      // Add a score entry for the new stage to all existing shooters
      setShooters(prev => prev.map(shooter => ({
        ...shooter,
        scores: [...shooter.scores, {
          stageId: newStage.id,
          rawTime: null,
          hits: { zone0: 0, zone2: 0, zone4: 0 },
          penalties: { FTI: false, TNN: 0, TNE: 0, HNT: 0, PE: 0, miss: 0 }
        }]
      })));

      showMessage('Stage added successfully!', 'success');
      return true;
    } catch (e) {
      console.error("Error adding stage: ", e);
      showMessage("Failed to add stage. Please try again.");
      return false;
    }
  };

  // Handle saving stage assignments (Admin only)
  const handleSaveAssignments = (assignments) => {
    setStages(prevStages => prevStages.map(stage => {
      const assignment = assignments.find(a => a.id === stage.id);
      return assignment ? { ...stage, rangeMarshal: assignment.rangeMarshal } : stage;
    }));
    showMessage('Stage assignments saved successfully!', 'success');
  };

  // Function to open the score input modal for a specific shooter
  const openScoreInput = (shooterId) => {
    const shooter = shooters.find(s => s.id === shooterId);
    if (!shooter) return;
    setEditingScoreForShooter(shooter);
    setShowScoreInput(true);
  };

  // Function to delete a shooter
  const handleDeleteShooter = (shooterId) => {
    if (!isAdmin) {
      showMessage('You do not have permission to delete shooters.', 'error');
      return;
    }
    // Replaced window.confirm with a custom modal logic (not fully implemented here)
    if (window.confirm('Are you sure you want to delete this shooter and all their data?')) {
      setShooters(prev => prev.filter(shooter => shooter.id !== shooterId));
      showMessage('Shooter deleted successfully.', 'success');
    }
  };

  // Function to delete a stage
  const handleDeleteStage = (stageId) => {
    if (!isAdmin) {
      showMessage('You do not have permission to delete stages.', 'error');
      return;
    }
    if (window.confirm('Are you sure you want to delete this stage and all associated scores?')) {
      setStages(prev => prev.filter(stage => stage.id !== stageId));
      setShooters(prev => prev.map(shooter => ({
        ...shooter,
        scores: shooter.scores.filter(score => score.stageId !== stageId)
      })));
      showMessage('Stage deleted successfully.', 'success');
    }
  };

  // Function to open the edit modal for a shooter
  const openEditShooterModal = (shooter) => {
    setEditingItem(shooter);
    setEditingItemType('shooter');
    setShowEditModal(true);
  };

  // Function to handle editing a shooter
  const handleEditShooter = (updatedShooter) => {
    setShooters(prev => prev.map(shooter =>
      shooter.id === updatedShooter.id ? updatedShooter : shooter
    ));
    showMessage('Shooter updated successfully!', 'success');
  };

  // Function to open the edit modal for a stage
  const openEditStageModal = (stage) => {
    setEditingItem(stage);
    setEditingItemType('stage');
    setShowEditModal(true);
  };

  // Function to handle editing a stage
  const handleEditStage = (updatedStage) => {
    setStages(prev => prev.map(stage =>
      stage.id === updatedStage.id ? updatedStage : stage
    ));
    showMessage('Stage updated successfully!', 'success');
  };

  // Function to edit a score directly (Admin only)
  const openEditScoreModal = (shooter) => {
      setEditingScoreForShooter(shooter);
      setShowScoreInput(true);
  };


  // Function to submit the score to the admin for approval
  const handleSubmitScore = async (shooterId, stageId, newScore) => {
    if (!loggedInRangeMarshal) {
      showMessage('Please log in as Range Marshal first.', 'error');
      return;
    }

    try {
      const newPendingScore = {
        id: generateId(),
        shooterId,
        stageId,
        ...newScore,
        isApproved: false,
        submittedBy: loggedInRangeMarshal,
        submittedAt: new Date(),
      };

      // Remove any existing pending score for this shooter/stage
      setPendingScores(prev => prev.filter(score =>
        !(score.shooterId === shooterId && score.stageId === stageId)
      ));

      // Add the new pending score
      setPendingScores(prev => [...prev, newPendingScore]);

      showMessage('Score submitted for approval!', 'success');
      setEditingScoreForShooter(null);
      setShowScoreInput(false);
    } catch (e) {
      console.error("Error submitting score: ", e);
      showMessage("Failed to submit score for approval. Please try again.");
    }
  };

  // Function to approve a pending score (Admin only)
  const handleApproveScore = (pendingScore) => {
    if (!isAdmin) return;

    try {
      // Update the shooter's score
      setShooters(prev => prev.map(shooter => {
        if (shooter.id === pendingScore.shooterId) {
          const updatedScores = shooter.scores.map(score =>
            score.stageId === pendingScore.stageId
              ? {
                ...score,
                rawTime: pendingScore.rawTime,
                hits: pendingScore.hits,
                penalties: pendingScore.penalties,
              }
              : score
          );

          // Calculate new overall score
          const newOverallScore = updatedScores.reduce((total, score) => {
            return total + calculateShooterScore(score).finalScore;
          }, 0);

          return {
            ...shooter,
            scores: updatedScores,
            overallScore: newOverallScore
          };
        }
        return shooter;
      }));

      // Remove the pending score
      setPendingScores(prev => prev.filter(score => score.id !== pendingScore.id));
      showMessage('Score approved and updated!', 'success');
    } catch (e) {
      console.error("Error approving score: ", e);
      showMessage("Failed to approve score. Please try again.", 'error');
    }
  };

  // Function to reject a pending score (Admin only)
  const handleRejectScore = (pendingScoreId) => {
    if (!isAdmin) return;
    setPendingScores(prev => prev.filter(score => score.id !== pendingScoreId));
    showMessage('Score rejected and removed from pending list.', 'success');
  };

  // Update leaderboard whenever shooters change
  useEffect(() => {
    const updatedLeaderboard = shooters
      .map(shooter => ({
        ...shooter,
        overallScore: shooter.scores?.reduce((total, score) => {
          return total + calculateShooterScore(score).finalScore;
        }, 0) || 0
      }))
      .sort((a, b) => (a.overallScore || 0) - (b.overallScore || 0));
    setLeaderboard(updatedLeaderboard);
  }, [shooters]);

  // Handle clicking on an item type to navigate to the form
  const handleItemClick = (type) => {
    setSelectedType(type);
  };

  // Custom message box component to replace alerts
  const MessageBox = ({ text, type }) => {
    if (!text) return null;
    return (
      <div className={`fixed bottom-4 right-4 p-4 rounded-lg shadow-xl z-50 transition-transform transform ${
          type === 'success' ? 'bg-green-500' : 'bg-red-500'
        } text-white`}>
        {text}
      </div>
    );
  };

  // Function to handle saving event details
  const handleEventDetailsSave = (updatedDetails) => {
    setEventDetails(updatedDetails);
    showMessage('Event details updated successfully!', 'success');
  };

  const assignedStage = stages.find(s => s.rangeMarshal === loggedInRangeMarshal);

  return (
    <div className="min-h-screen bg-gray-900 text-gray-100 font-inter">
      <header className="bg-gray-800 p-4 shadow-lg sticky top-0 z-40">
        <div className="container mx-auto flex justify-between items-center">
          <div className="flex items-center space-x-4">
            <Award size={32} className="text-cyan-400" />
            <h1 className="text-3xl font-extrabold text-white hidden sm:block">CADHA SCORING SYSTEM</h1>
            {/* Logged-in status indicators */}
            {isAdmin && (
              <span className="bg-red-600 text-white text-xs font-bold px-3 py-1 rounded-full uppercase ml-4 hidden sm:block">
                Admin
              </span>
            )}
            {loggedInRangeMarshal && (
              <span className="bg-green-600 text-white text-xs font-bold px-3 py-1 rounded-full ml-4 hidden sm:block">
                RM: {loggedInRangeMarshal}
              </span>
            )}
          </div>
          <nav className="flex items-center space-x-4">
            {(isAdmin || loggedInRangeMarshal) && (
              <button
                onClick={handleRangeMarshalLogout} // Log out both Admin and RM
                className="bg-red-600 hover:bg-red-700 text-white font-bold py-2 px-4 rounded-lg flex items-center transition-colors"
              >
                <LogOut size={18} className="mr-2" />
                Logout
              </button>
            )}
          </nav>
        </div>
      </header>

      <main className="container mx-auto mt-8 p-4">
        {currentView === 'login' && (
          <div className="flex flex-col lg:flex-row gap-8 justify-center items-center lg:items-start p-8">
            <div className="bg-gray-800 p-8 rounded-2xl shadow-2xl w-full max-w-sm">
              <h2 className="text-3xl font-bold mb-6 text-center text-cyan-400">Admin Login</h2>
              <form onSubmit={handleAdminLogin} className="space-y-4">
                <div>
                  <label htmlFor="admin-code" className="block text-sm font-medium text-gray-300">Admin Code</label>
                  <input
                    id="admin-code"
                    type="password"
                    value={adminCode}
                    onChange={(e) => setAdminCode(e.target.value)}
                    className="w-full p-3 mt-1 bg-gray-700 rounded-lg border border-gray-600 focus:border-cyan-400 focus:outline-none text-white"
                  />
                </div>
                {loginError && <p className="text-red-400 text-sm text-center">{loginError}</p>}
                <button
                  type="submit"
                  className="w-full bg-cyan-500 hover:bg-cyan-600 text-white font-bold py-3 rounded-lg flex items-center justify-center transition-colors"
                >
                  <LogIn size={20} className="mr-2" />
                  Log In as Admin
                </button>
              </form>
            </div>

            <div className="bg-gray-800 p-8 rounded-2xl shadow-2xl w-full max-w-sm">
              <h2 className="text-3xl font-bold mb-6 text-center text-yellow-400">Range Marshal Login</h2>
              <div className="space-y-4">
                <div>
                  <label htmlFor="range-marshal-name" className="block text-sm font-medium text-gray-300">Your Name</label>
                  <input
                    id="range-marshal-name"
                    type="text"
                    value={rangeMarshalLogin}
                    onChange={(e) => setRangeMarshalLogin(e.target.value)}
                    className="w-full p-3 mt-1 bg-gray-700 rounded-lg border border-gray-600 focus:border-yellow-400 focus:outline-none text-white"
                    placeholder="Enter your name"
                  />
                </div>
                <button
                  onClick={handleRangeMarshalLogin}
                  className="w-full bg-yellow-500 hover:bg-yellow-600 text-gray-900 font-bold py-3 rounded-lg flex items-center justify-center transition-colors"
                >
                  <User size={20} className="mr-2" />
                  Log In as Range Marshal
                </button>
              </div>
            </div>
          </div>
        )}

        {currentView === 'dashboard' && (
          <div className="space-y-8">
             {/* Event Details Component */}
             <EventDetails
               eventDetails={eventDetails}
               onSave={handleEventDetailsSave}
               isAdmin={isAdmin}
             />

            {isAdmin && (
              <div className="bg-gray-800 p-6 rounded-xl shadow-lg flex flex-wrap justify-center sm:justify-start gap-4">
                <button
                  onClick={() => handleItemClick('shooter')}
                  className="flex-1 min-w-[200px] bg-green-500 hover:bg-green-600 text-white font-bold py-3 px-6 rounded-lg transition-colors flex items-center justify-center"
                >
                  <Plus size={20} className="mr-2" />
                  Add Shooter
                </button>
                <button
                  onClick={() => setShowAssignStagesModal(true)}
                  className="flex-1 min-w-[200px] bg-indigo-500 hover:bg-indigo-600 text-white font-bold py-3 px-6 rounded-lg transition-colors flex items-center justify-center"
                >
                  <ClipboardCheck size={20} className="mr-2" />
                  Assign/Add Stages
                </button>
                <button
                  onClick={() => setShowAdminApproval(true)}
                  className="flex-1 min-w-[200px] bg-yellow-500 hover:bg-yellow-600 text-gray-900 font-bold py-3 px-6 rounded-lg transition-colors flex items-center justify-center"
                >
                  <UserCheck size={20} className="mr-2" />
                  Review Scores
                </button>
              </div>
            )}

            {loggedInRangeMarshal && (
              <div className="bg-gray-800 p-6 rounded-xl shadow-lg text-center">
                <h3 className="text-xl font-bold text-yellow-400">Range Marshal Actions</h3>
                {assignedStage ? (
                  <p className="text-gray-300 mt-2">
                    Your assigned stage is: <span className="font-bold text-white">{assignedStage.name}</span>.
                    You can only score for this stage.
                  </p>
                ) : (
                  <p className="text-gray-300 mt-2">
                    No stage has been assigned to you yet. Please check with the admin.
                  </p>
                )}
              </div>
            )}

            {/* Shooters List Section */}
            <div className="bg-gray-800 p-6 rounded-xl shadow-lg">
              <h3 className="text-xl font-bold text-gray-100 mb-4 flex items-center">
                <UserCheck size={24} className="mr-2 text-cyan-400" /> Shooters ({shooters.length})
              </h3>
              <div className="overflow-x-auto">
                <table className="min-w-full divide-y divide-gray-700">
                  <thead className="bg-gray-700">
                    <tr>
                      <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                        Name
                      </th>
                      <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                        Division
                      </th>
                      <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                        Category
                      </th>
                      <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                        Actions
                      </th>
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-gray-700">
                    {shooters.map(shooter => (
                      <tr key={shooter.id} className="hover:bg-gray-700 transition-colors">
                        <td className="px-6 py-4 whitespace-nowrap text-sm font-medium text-white">{shooter.name}</td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-300">{shooter.division}</td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-300">{shooter.category}</td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm font-medium">
                          <div className="flex space-x-2">
                            {isAdmin && (
                              <>
                                <button
                                  onClick={() => openEditShooterModal(shooter)}
                                  className="text-cyan-400 hover:text-cyan-600 transition-colors"
                                  title="Edit Shooter"
                                >
                                  <Edit size={20} />
                                </button>
                                <button
                                  onClick={() => handleDeleteShooter(shooter.id)}
                                  className="text-red-400 hover:text-red-600 transition-colors"
                                  title="Delete Shooter"
                                >
                                  <Trash2 size={20} />
                                </button>
                              </>
                            )}
                            {loggedInRangeMarshal && assignedStage && (
                              <button
                                onClick={() => openScoreInput(shooter.id)}
                                className="bg-yellow-500 hover:bg-yellow-600 text-gray-900 font-bold py-1 px-3 rounded-md text-xs transition-colors"
                              >
                                Score
                              </button>
                            )}
                            {loggedInRangeMarshal && !assignedStage && (
                              <span className="text-sm text-gray-500 italic">No stage assigned</span>
                            )}
                          </div>
                        </td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
            </div>

            {/* Stages List Section */}
            <div className="bg-gray-800 p-6 rounded-xl shadow-lg">
              <h3 className="text-xl font-bold text-gray-100 mb-4 flex items-center">
                <Target size={24} className="mr-2 text-cyan-400" /> Stages ({stages.length})
              </h3>
              <div className="overflow-x-auto">
                <table className="min-w-full divide-y divide-gray-700">
                  <thead className="bg-gray-700">
                    <tr>
                      <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                        Stage Name
                      </th>
                      <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                        Range Marshal
                      </th>
                      <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                        Targets
                      </th>
                      <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                        Rounds
                      </th>
                      {isAdmin && (
                        <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                          Actions
                        </th>
                      )}
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-gray-700">
                    {stages.map(stage => (
                      <tr key={stage.id} className="hover:bg-gray-700 transition-colors">
                        <td className="px-6 py-4 whitespace-nowrap text-sm font-medium text-white">{stage.name}</td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-300">{stage.rangeMarshal || 'Unassigned'}</td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-300">
                          Threat: {stage.threatTargets}, Non-Threat: {stage.nonThreatTargets}
                        </td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-300">{stage.totalRounds}</td>
                        {isAdmin && (
                          <td className="px-6 py-4 whitespace-nowrap text-sm font-medium">
                            <div className="flex space-x-2">
                              <button
                                onClick={() => openEditStageModal(stage)}
                                className="text-cyan-400 hover:text-cyan-600 transition-colors"
                                title="Edit Stage"
                              >
                                <Edit size={20} />
                              </button>
                              <button
                                onClick={() => handleDeleteStage(stage.id)}
                                className="text-red-400 hover:text-red-600 transition-colors"
                                title="Delete Stage"
                              >
                                <Trash2 size={20} />
                              </button>
                            </div>
                          </td>
                        )}
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
            </div>

            {/* Leaderboard Section */}
            <div className="bg-gray-800 p-6 rounded-xl shadow-lg">
              <h3 className="text-xl font-bold text-gray-100 mb-4 flex items-center">
                <Award size={24} className="mr-2 text-yellow-400" /> Leaderboard
              </h3>
              <div className="overflow-x-auto">
                <table className="min-w-full divide-y divide-gray-700">
                  <thead className="bg-gray-700">
                    <tr>
                      <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                        Rank
                      </th>
                      <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                        Shooter Name
                      </th>
                      <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-300 uppercase tracking-wider">
                        Overall Score
                      </th>
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-gray-700">
                    {leaderboard.map((shooter, index) => (
                      <tr key={shooter.id} className="hover:bg-gray-700 transition-colors">
                        <td className="px-6 py-4 whitespace-nowrap text-sm font-medium text-white">{index + 1}</td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-300">{shooter.name}</td>
                        <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-300">
                          {shooter.overallScore ? shooter.overallScore.toFixed(2) : 'N/A'}
                        </td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        )}
      </main>

      {/* Conditional rendering for modals */}
      {showAdminApproval && isAdmin && (
        <Modal title="Pending Score Approvals" onClose={() => setShowAdminApproval(false)}>
          {pendingScores.length > 0 ? (
            <div className="space-y-4">
              {pendingScores.map(score => (
                <div key={score.id} className="bg-gray-700 p-4 rounded-lg flex items-center justify-between">
                  <div>
                    <p className="text-sm font-bold text-white">{shooters.find(s => s.id === score.shooterId)?.name || 'Unknown Shooter'}</p>
                    <p className="text-xs text-gray-400">{getStageDetails(score.stageId).name}</p>
                    <p className="text-xs text-gray-400 mt-1">Submitted by: {score.submittedBy}</p>
                  </div>
                  <div className="flex space-x-2">
                    <button
                      onClick={() => handleApproveScore(score)}
                      className="p-2 bg-green-500 hover:bg-green-600 text-white rounded-md transition-colors"
                      title="Approve"
                    >
                      <CheckCircle size={20} />
                    </button>
                    <button
                      onClick={() => handleRejectScore(score.id)}
                      className="p-2 bg-red-500 hover:bg-red-600 text-white rounded-md transition-colors"
                      title="Reject"
                    >
                      <Ban size={20} />
                    </button>
                  </div>
                </div>
              ))}
            </div>
          ) : (
            <p className="text-center text-gray-400">No scores pending approval.</p>
          )}
        </Modal>
      )}
      {showScoreInput && (editingScoreForShooter || isAdmin) && (
        <ScoringModal
          shooter={editingScoreForShooter}
          stages={stages}
          onClose={() => setShowScoreInput(false)}
          onSubmitScore={handleSubmitScore}
          loggedInRangeMarshal={loggedInRangeMarshal}
        />
      )}
      {showEditModal && isAdmin && (
        <EditModal
          item={editingItem}
          type={editingItemType}
          onClose={() => setShowEditModal(false)}
          onEdit={editingItemType === 'shooter' ? handleEditShooter : handleEditStage}
        />
      )}
      {selectedType && isAdmin && (
        <Modal
          title={`Add New ${selectedType}`}
          onClose={() => setSelectedType(null)}
        >
          <InputForm
            type={selectedType}
            onSubmit={selectedType === 'shooter' ? handleAddShooter : handleAddStage}
            onCancel={() => setSelectedType(null)}
          />
        </Modal>
      )}
      {showAssignStagesModal && isAdmin && (
        <AssignStagesModal
          stages={stages}
          onSave={handleSaveAssignments}
          onCancel={() => setShowAssignStagesModal(false)}
          onAddStage={handleAddStage}
        />
      )}

      {/* Message Box for notifications */}
      <MessageBox text={message.text} type={message.type} />
    </div>
  );
};

export default App;

