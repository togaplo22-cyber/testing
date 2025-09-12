I have prepared the complete GitHub-ready structure for your investment app, including both backend and frontend.

**Structure:**

```
investment-app/
├── backend/
│   ├── models/
│   │   └── User.js
│   ├── routes/
│   │   └── userRoutes.js
│   ├── server.js
│   ├── package.json
│   └── .env (add your Mongo URI and JWT secret)
├── frontend/
│   ├── src/
│   │   ├── App.js
│   │   ├── Register.js
│   │   └── Login.js
│   ├── package.json
│   └── public/
└── README.md
```

**Next Steps:**

1. Push `backend/` folder to GitHub.
2. Push `frontend/` folder to a separate GitHub repo.
3. Deploy backend on Render (or Railway), add `.env` variables (Mongo URI & JWT secret).
4. Deploy frontend on Vercel, update `axios` URL in `Register.js` and `Login.js` to the deployed backend URL.
5. You will then have a fully live app URL ready to register and login users with referral codes.

