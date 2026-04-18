title Target-State Survey Update Architecture

  CNA [icon: file-text, label: "CNA Guidelines PDF"]

  Internal Team [icon: users] {
    Workstation [icon: monitor] {
      Local Question DB [icon: database, label: "Local Question Bank DB"]
      Survey Management App [icon: app-window, label: "Survey Management App"]
    }
  }

  Internet [icon: globe]

  Microsoft 365 Cloud [icon: cloud, color: blue] {
    OneDrive [icon: microsoft-onedrive, label: "OneDrive Sync Folder"]
    Synced DB File [icon: file, label: "Synced Database File"]
    Approved Snapshots [icon: archive, label: "Approved Version Snapshots"]
  }

  Survey Provider [icon: server, color: orange] {
    Provider Platform [icon: browser, label: "Survey Provider Platform"]
    Survey Links [icon: link]
    Comparative Reports [icon: chart-column, label: "Comparative Reports"]
  }

  CNA > Workstation
  Survey Management App <> Local Question DB

  Local Question DB <> Synced DB File
  Synced DB File > OneDrive
  OneDrive > Approved Snapshots

  Workstation > Internet
  Internet > OneDrive
  Internet > Provider Platform

  Local Question DB > Provider Platform
  Provider Platform > Survey Links
  Provider Platform > Comparative Reports

  Survey Links > Workstation
  Comparative Reports > Workstation
