# home

git init
git remote add origin https://github.com/Gokul79618/hostel.git
git add .
git commit -m "frontend"
git push -u origin master

cd hos
call npm ci || call npm install
call npm run build 
mkdir "C:\ProgramData\Jenkins\.Jenkins\userContent\react-app"
xcopy /E /I /Y "dist\*" "C:\ProgramData\Jenkins\.Jenkins\userContent\react-app\"


pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Muthukumaran17/hostel.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // If your project files are inside a folder named 'hos', keep this
                // Otherwise, remove the dir('hos') wrapper
                dir('hostel') {
                    bat '"C:\\Program Files\\nodejs\\npm.cmd" install'
                }
            }
        }

        stage('Build') {
            steps {
                dir('hostel') {
                    bat '"C:\\Program Files\\nodejs\\npm.cmd" run build'
                }
            }
        }
    }

    post {
        success {
            echo "✅ React project built successfully!"
        }
        failure {
            echo "❌ Build failed!"
        }
    }
}


npx serve -s . -l 8080


#docker

FROM node:18-alpine AS build
# Set working directory inside container
WORKDIR /app
# Copy package.json and package-lock.json (if available)
COPY package*.json ./
# Install dependencies
RUN npm install
# Copy the rest of the app source code
COPY . .
RUN npm run build
# Stage 2: Serve the app using Nginx
FROM nginx:alpine
# Copy build output from previous stage to Nginx's HTML directory
COPY --from=build /app/dist /usr/share/nginx/html
# Expose port 80 inside the container
EXPOSE 80
# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]  

dockerfile


node_modules
build
.git
Dockerfile
.dockerignore

.dockerignore



docker pull node:18-alpine
docker pull nginx:alpine
docker build -t filename .
docker run -d -p 3000:80 filename
git init
git add .
git commit -m "hi"
git remote add origin 
git branch 
git branch -m main
git push -u origin main


Form validation

import { useState } from "react";

function App() {
  const [formData, setFormData] = useState({ name: "", email: "", password: "" });
  const [errors, setErrors] = useState({});

  const handleChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const validate = () => {
    let newErrors = {};
    if (!formData.name.trim()) newErrors.name = "Name is required";
    if (!/\S+@\S+\.\S+/.test(formData.email)) newErrors.email = "Invalid email";
    if (formData.password.length < 6)
      newErrors.password = "Password must be at least 6 characters";
    return newErrors;
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const validationErrors = validate();
    if (Object.keys(validationErrors).length === 0) {
      alert("Form submitted successfully!");
    } else {
      setErrors(validationErrors);
    }
  };

  return (
    <div style={{ padding: 20, maxWidth: 400, margin: "auto" }}>
      <h2>Form Validation</h2>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          name="name"
          placeholder="Enter name"
          value={formData.name}
          onChange={handleChange}
        />
        <p style={{ color: "red" }}>{errors.name}</p>

        <input
          type="email"
          name="email"
          placeholder="Enter email"
          value={formData.email}
          onChange={handleChange}
        />
        <p style={{ color: "red" }}>{errors.email}</p>

        <input
          type="password"
          name="password"
          placeholder="Enter password"
          value={formData.password}
          onChange={handleChange}
        />
        <p style={{ color: "red" }}>{errors.password}</p>

        <button type="submit">Submit</button>
      </form>
    </div>
  );
}

export default App;


BMI calc

import { useState } from "react";

function App() {
  const [weight, setWeight] = useState("");
  const [height, setHeight] = useState("");
  const [bmi, setBmi] = useState(null);

  const calculateBMI = () => {
    if (weight && height) {
      const result = (weight / (height * height)).toFixed(2);
      setBmi(result);
    } else {
      alert("Please enter valid weight and height");
    }
  };

  return (
    <div style={{ padding: 20, textAlign: "center" }}>
      <h2>BMI Calculator</h2>
      <input
        type="number"
        placeholder="Weight (kg)"
        value={weight}
        onChange={(e) => setWeight(e.target.value)}
      />
      <br /><br />
      <input
        type="number"
        placeholder="Height (m)"
        value={height}
        onChange={(e) => setHeight(e.target.value)}
      />
      <br /><br />
      <button onClick={calculateBMI}>Calculate BMI</button>
      {bmi && (
        <h3>
          Your BMI: {bmi}{" "}
          {bmi < 18.5
            ? "(Underweight)"
            : bmi < 25
            ? "(Normal)"
            : bmi < 30
            ? "(Overweight)"
            : "(Obese)"}
        </h3>
      )}
    </div>
  );
}

export default App;


CRUD

import React, { useState } from "react";

export default function App() {
  const [items, setItems] = useState([]);
  const [input, setInput] = useState("");
  const [editIndex, setEditIndex] = useState(null);

  const handleAdd = () => {
    if (input.trim() === "") return;

    if (editIndex !== null) {
      const updatedItems = [...items];
      updatedItems[editIndex] = input;
      setItems(updatedItems);
      setEditIndex(null);
    } else {
      setItems([...items, input]);
    }

    setInput("");
  };

  const handleEdit = (index) => {
    setInput(items[index]);
    setEditIndex(index);
  };

  const handleDelete = (index) => {
    setItems(items.filter((_, i) => i !== index));
  };

  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <h2>Simple CRUD Application</h2>
      <input
        type="text"
        placeholder="Enter item"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        style={{ padding: "8px", width: "200px" }}
      />
      <button onClick={handleAdd} style={{ marginLeft: "10px", padding: "8px" }}>
        {editIndex !== null ? "Update" : "Add"}
      </button>

      <ul style={{ listStyle: "none", padding: 0, marginTop: "20px" }}>
        {items.map((item, index) => (
          <li key={index} style={{ marginBottom: "10px" }}>
            {item}
            <button
              onClick={() => handleEdit(index)}
              style={{ marginLeft: "10px", padding: "5px" }}
            >
              Edit
            </button>
            <button
              onClick={() => handleDelete(index)}
              style={{ marginLeft: "5px", padding: "5px" }}
            >
              Delete
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

API


import React, { useEffect, useState } from "react";

export default function App() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  // Fetch data from API when component mounts
  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((data) => {
        setUsers(data);
        setLoading(false);
      })
      .catch((error) => {
        console.error("Error fetching data:", error);
        setLoading(false);
      });
  }, []);

  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <h2>Simple API Fetch Example</h2>

      {loading ? (
        <p>Loading data...</p>
      ) : (
        <ul style={{ listStyle: "none", padding: 0 }}>
          {users.map((user) => (
            <li
              key={user.id}
              style={{
                margin: "10px auto",
                width: "300px",
                border: "1px solid #ccc",
                borderRadius: "8px",
                padding: "10px",
              }}
            >
              <strong>{user.name}</strong>
              <br />
              📧 {user.email}
              <br />
              🏠 {user.address.city}
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}

