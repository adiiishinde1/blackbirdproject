# blackbirdproject
import React, { useState } from 'react';
import TextField from '@mui/material/TextField';
import Button from '@mui/material/Button';
import Snackbar from '@mui/material/Snackbar';
import MuiAlert from '@mui/material/Alert';
import emailValidator from 'email-validator';

const LoginForm = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [emailError, setEmailError] = useState('');
  const [passwordError, setPasswordError] = useState('');
  const [successMessage, setSuccessMessage] = useState('');
  const [errorMessage, setErrorMessage] = useState('');

  const handleEmailChange = (event) => {
    setEmail(event.target.value);
  };

  const handlePasswordChange = (event) => {
    setPassword(event.target.value);
  };

  const handleFormSubmit = (event) => {
    event.preventDefault();

    // Validate email
    if (!emailValidator.validate(email)) {
      setEmailError('Invalid email');
      return;
    }

    // Validate password
    const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/;
    if (!passwordRegex.test(password)) {
      setPasswordError(
        'Password must be at least 8 characters long and contain at least one uppercase letter, one lowercase letter, one numeric digit, and one special character.'
      );
      return;
    }

    // Validation passed, show success message
    setSuccessMessage('Validation successful');
    setErrorMessage('');
  };

  const handleSnackbarClose = () => {
    setSuccessMessage('');
    setErrorMessage('');
  };

  return (
    <form onSubmit={handleFormSubmit}>
      <TextField
        label="Email"
        type="email"
        value={email}
        onChange={handleEmailChange}
        error={!!emailError}
        helperText={emailError}
      />
      <TextField
        label="Password"
        type="password"
        value={password}
        onChange={handlePasswordChange}
        error={!!passwordError}
        helperText={passwordError}
      />
      <Button type="submit" variant="contained" color="primary">
        Submit
      </Button>
      <Snackbar open={!!successMessage || !!errorMessage} autoHideDuration={3000} onClose={handleSnackbarClose}>
        {successMessage && (
          <MuiAlert severity="success" onClose={handleSnackbarClose}>
            {successMessage}
          </MuiAlert>
        )}
        {errorMessage && (
          <MuiAlert severity="error" onClose={handleSnackbarClose}>
            {errorMessage}
          </MuiAlert>
        )}
      </Snackbar>
    </form>
  );
};

export default LoginForm;
