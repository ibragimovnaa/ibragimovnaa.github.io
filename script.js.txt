// Light / dark theme switch
const themeBtn = document.getElementById("themeBtn");
themeBtn.addEventListener("click", () => {
  document.body.classList.toggle("dark");
  themeBtn.textContent = document.body.classList.contains("dark") ? "Light mode" : "Dark mode";
});

// Mobile navigation: close it after choosing a section.
const menuToggle = document.getElementById("menuToggle");
const mainNav = document.getElementById("mainNav");
menuToggle.addEventListener("click", () => {
  const isOpen = mainNav.classList.toggle("open");
  menuToggle.setAttribute("aria-expanded", String(isOpen));
  menuToggle.setAttribute("aria-label", isOpen ? "Close navigation menu" : "Open navigation menu");
});
mainNav.querySelectorAll("a").forEach(link => {
  link.addEventListener("click", () => {
    mainNav.classList.remove("open");
    menuToggle.setAttribute("aria-expanded", "false");
    menuToggle.setAttribute("aria-label", "Open navigation menu");
  });
});

// Show the provider / ASN name only. Never render the IP address.
const ispName = document.getElementById("ispName");
const asnName = document.getElementById("asnName");
fetch("https://ipwho.is/")
  .then(response => {
    if (!response.ok) throw new Error("Network lookup failed");
    return response.json();
  })
  .then(data => {
    if (!data.success) throw new Error("Network lookup unavailable");
    const connection = data.connection || {};
    const provider = connection.isp || connection.org || "Not available";
    ispName.textContent = provider;
    asnName.textContent = connection.asn
      ? `Autonomous System Number: ${connection.asn}`
      : "Provider information detected from this connection.";
  })
  .catch(() => {
    ispName.textContent = "Provider lookup unavailable";
    asnName.textContent = "The network information service could not be reached.";
  });
